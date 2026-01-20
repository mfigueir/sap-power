# SAP CAP Development Guide

Comprehensive guide for building SAP Cloud Application Programming Model (CAP) applications using the SAP Skills Power.

## Overview

The `sap-cap-capire` skill provides expert guidance for CAP development, covering:
- CDS modeling
- Service implementation
- Authentication & authorization
- Database integration
- BTP deployment

## CDS Modeling Best Practices

### Entity Definitions

```cds
namespace my.bookshop;

// Use managed aspects for common fields
entity Books : managed {
  key ID : UUID;
  title  : String(111);
  author : Association to Authors;
  stock  : Integer;
  price  : Decimal(9,2);
}

entity Authors : managed {
  key ID : UUID;
  name   : String(111);
  books  : Association to many Books on books.author = $self;
}

// Managed aspect provides:
// - createdAt, createdBy
// - modifiedAt, modifiedBy
```

### Annotations

```cds
// Add UI annotations for Fiori Elements
annotate Books with @(
  UI.LineItem: [
    {Value: title, Label: 'Title'},
    {Value: author.name, Label: 'Author'},
    {Value: stock, Label: 'Stock'},
    {Value: price, Label: 'Price'}
  ],
  UI.HeaderInfo: {
    TypeName: 'Book',
    TypeNamePlural: 'Books',
    Title: {Value: title}
  }
);

// Add validation
annotate Books with {
  title @mandatory;
  stock @assert.range: [0, 999];
  price @assert.range: [0.01, 9999.99];
}
```

### Service Definitions

```cds
service CatalogService {
  // Expose entities with projections
  entity Books as projection on bookshop.Books;
  entity Authors as projection on bookshop.Authors;
  
  // Add custom actions
  action submitOrder(book: UUID, quantity: Integer) returns {
    stock: Integer;
    message: String;
  };
  
  // Add custom functions
  function getAvailableBooks() returns array of Books;
}
```

## Service Implementation

### Basic Handler

```javascript
// srv/service.js
const cds = require('@sap/cds');

module.exports = cds.service.impl(async function() {
  const { Books, Authors } = this.entities;
  
  // Before CREATE validation
  this.before('CREATE', 'Books', async (req) => {
    const { stock, price } = req.data;
    if (stock < 0) throw new Error('Stock cannot be negative');
    if (price <= 0) throw new Error('Price must be positive');
  });
  
  // After READ enrichment
  this.after('READ', 'Books', (books) => {
    books.forEach(book => {
      book.availability = book.stock > 0 ? 'In Stock' : 'Out of Stock';
    });
  });
  
  // Custom action implementation
  this.on('submitOrder', async (req) => {
    const { book, quantity } = req.data;
    
    // Get book with lock
    const bookRecord = await SELECT.one.from(Books)
      .where({ ID: book })
      .forUpdate();
    
    if (!bookRecord) throw new Error('Book not found');
    if (bookRecord.stock < quantity) {
      throw new Error('Insufficient stock');
    }
    
    // Update stock
    await UPDATE(Books)
      .set({ stock: bookRecord.stock - quantity })
      .where({ ID: book });
    
    return {
      stock: bookRecord.stock - quantity,
      message: 'Order submitted successfully'
    };
  });
});
```

### Advanced Patterns

#### Draft Support

```cds
service CatalogService {
  @odata.draft.enabled
  entity Books as projection on bookshop.Books;
}
```

```javascript
// Draft handlers are automatic, but you can customize:
this.before('SAVE', 'Books', async (req) => {
  // Validate before activating draft
  const { title, author } = req.data;
  if (!title || !author) {
    throw new Error('Title and author are required');
  }
});
```

#### Fiori Draft Support

```javascript
// srv/service.js
this.on('draftPrepare', 'Books', async (req) => {
  // Custom logic before draft preparation
  console.log('Preparing draft for book:', req.data.ID);
});

this.on('draftActivate', 'Books', async (req) => {
  // Custom logic before draft activation
  const book = req.data;
  // Perform final validations
  if (book.stock < 0) {
    req.error(400, 'Stock cannot be negative');
  }
});
```

## Authentication & Authorization

### Define Roles

```json
// xs-security.json
{
  "xsappname": "bookshop",
  "tenant-mode": "dedicated",
  "scopes": [
    {
      "name": "$XSAPPNAME.Admin",
      "description": "Administrator"
    },
    {
      "name": "$XSAPPNAME.User",
      "description": "Regular User"
    }
  ],
  "role-templates": [
    {
      "name": "Admin",
      "description": "Administrator",
      "scope-references": ["$XSAPPNAME.Admin"]
    },
    {
      "name": "User",
      "description": "Regular User",
      "scope-references": ["$XSAPPNAME.User"]
    }
  ]
}
```

### Apply Authorization

```cds
// srv/service.cds
service CatalogService @(requires: 'authenticated-user') {
  
  // Admin-only operations
  entity Books @(restrict: [
    { grant: ['READ'], to: 'User' },
    { grant: ['*'], to: 'Admin' }
  ]) as projection on bookshop.Books;
  
  // Public read, authenticated write
  entity Authors @(restrict: [
    { grant: 'READ' },
    { grant: ['CREATE', 'UPDATE', 'DELETE'], to: 'Admin' }
  ]) as projection on bookshop.Authors;
}
```

### Instance-Based Authorization

```javascript
// srv/service.js
this.before('READ', 'Books', async (req) => {
  const user = req.user;
  
  // Filter based on user attributes
  if (!user.is('Admin')) {
    req.query.where({ stock: { '>': 0 } });
  }
});
```

## Database Integration

### SQLite (Development)

```json
// package.json
{
  "cds": {
    "requires": {
      "db": {
        "kind": "sqlite",
        "credentials": {
          "database": "db.sqlite"
        }
      }
    }
  }
}
```

### SAP HANA (Production)

```json
// package.json
{
  "cds": {
    "requires": {
      "db": {
        "kind": "hana",
        "deploy-format": "hdbtable"
      }
    }
  }
}
```

### PostgreSQL

```json
// package.json
{
  "cds": {
    "requires": {
      "db": {
        "kind": "postgres",
        "credentials": {
          "host": "localhost",
          "port": 5432,
          "database": "bookshop",
          "user": "postgres",
          "password": "postgres"
        }
      }
    }
  }
}
```

## Testing

### Unit Tests

```javascript
// test/service.test.js
const cds = require('@sap/cds/lib');
const { expect } = require('chai');

describe('CatalogService', () => {
  let srv;
  
  before(async () => {
    srv = await cds.connect.to('CatalogService');
  });
  
  it('should create a book', async () => {
    const book = await srv.create('Books', {
      title: 'Test Book',
      stock: 10,
      price: 29.99
    });
    
    expect(book).to.have.property('ID');
    expect(book.title).to.equal('Test Book');
  });
  
  it('should reject negative stock', async () => {
    try {
      await srv.create('Books', {
        title: 'Invalid Book',
        stock: -5,
        price: 29.99
      });
      expect.fail('Should have thrown error');
    } catch (err) {
      expect(err.message).to.include('Stock cannot be negative');
    }
  });
});
```

### Integration Tests

```javascript
// test/integration.test.js
const cds = require('@sap/cds/lib');
const { GET, POST, DELETE } = cds.test(__dirname + '/..');

describe('OData API', () => {
  it('should GET books', async () => {
    const { data } = await GET('/catalog/Books');
    expect(data.value).to.be.an('array');
  });
  
  it('should POST a new book', async () => {
    const { data } = await POST('/catalog/Books', {
      title: 'New Book',
      stock: 5,
      price: 19.99
    });
    expect(data).to.have.property('ID');
  });
});
```

## BTP Deployment

### MTA Descriptor

```yaml
# mta.yaml
_schema-version: '3.3'
ID: bookshop
version: 1.0.0

modules:
  - name: bookshop-srv
    type: nodejs
    path: gen/srv
    requires:
      - name: bookshop-db
      - name: bookshop-auth
    provides:
      - name: srv-api
        properties:
          srv-url: ${default-url}

  - name: bookshop-db-deployer
    type: hdb
    path: gen/db
    requires:
      - name: bookshop-db

resources:
  - name: bookshop-db
    type: com.sap.xs.hdi-container
    properties:
      hdi-service-name: ${service-name}

  - name: bookshop-auth
    type: org.cloudfoundry.managed-service
    parameters:
      service: xsuaa
      service-plan: application
      path: ./xs-security.json
```

### Build and Deploy

```bash
# Build MTA
mbt build

# Deploy to BTP
cf deploy mta_archives/bookshop_1.0.0.mtar

# Check status
cf apps
cf services
```

## Performance Optimization

### Efficient Queries

```javascript
// Bad: N+1 queries
const books = await SELECT.from(Books);
for (const book of books) {
  book.author = await SELECT.one.from(Authors).where({ ID: book.author_ID });
}

// Good: Single query with expand
const books = await SELECT.from(Books).columns(b => {
  b('*'),
  b.author(a => a('*'))
});
```

### Pagination

```javascript
// Automatic pagination
this.on('READ', 'Books', async (req) => {
  // CAP handles $top and $skip automatically
  // But you can customize:
  const { limit, offset } = req.query.SELECT;
  console.log(`Fetching ${limit} books starting at ${offset}`);
});
```

### Caching

```javascript
const cache = new Map();

this.on('READ', 'Authors', async (req, next) => {
  const cacheKey = JSON.stringify(req.query);
  
  if (cache.has(cacheKey)) {
    return cache.get(cacheKey);
  }
  
  const result = await next();
  cache.set(cacheKey, result);
  
  return result;
});
```

## Error Handling

```javascript
// Custom error class
class BusinessError extends Error {
  constructor(message, code = 400) {
    super(message);
    this.code = code;
  }
}

// Use in handlers
this.on('submitOrder', async (req) => {
  const { book, quantity } = req.data;
  
  if (quantity <= 0) {
    throw new BusinessError('Quantity must be positive', 400);
  }
  
  const bookRecord = await SELECT.one.from(Books).where({ ID: book });
  
  if (!bookRecord) {
    throw new BusinessError('Book not found', 404);
  }
  
  if (bookRecord.stock < quantity) {
    throw new BusinessError('Insufficient stock', 409);
  }
  
  // Process order...
});
```

## Best Practices Summary

1. **Use Managed Aspects**: Leverage `managed`, `cuid`, `temporal` for common fields
2. **Validate Early**: Use `@assert` annotations and `before` handlers
3. **Secure by Default**: Always define authorization rules
4. **Test Thoroughly**: Write unit and integration tests
5. **Optimize Queries**: Use projections and avoid N+1 queries
6. **Handle Errors**: Provide meaningful error messages
7. **Document APIs**: Use annotations for Fiori Elements
8. **Follow Conventions**: Use CAP naming and structure patterns

## Resources

- [CAP Documentation](https://cap.cloud.sap/docs/)
- [CDS Language Reference](https://cap.cloud.sap/docs/cds/)
- [CAP Node.js Runtime](https://cap.cloud.sap/docs/node.js/)
- [CAP Samples](https://github.com/SAP-samples/cloud-cap-samples)
