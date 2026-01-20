# SAP Development Best Practices

Essential best practices for SAP development across all technologies in the SAP ecosystem.

## General Principles

### 1. Follow SAP Standards

- Use official SAP naming conventions
- Follow SAP design patterns
- Leverage SAP frameworks and libraries
- Stay current with SAP recommendations

### 2. Security First

- Implement authentication and authorization
- Validate all inputs
- Use parameterized queries
- Encrypt sensitive data
- Follow OWASP guidelines

### 3. Performance Optimization

- Minimize database queries
- Use pagination for large datasets
- Implement caching strategies
- Optimize OData queries with $select and $expand
- Use asynchronous operations

### 4. Code Quality

- Write clean, readable code
- Add comprehensive comments
- Follow DRY (Don't Repeat Yourself)
- Use meaningful variable names
- Implement error handling

### 5. Testing

- Write unit tests
- Implement integration tests
- Test edge cases
- Automate testing
- Maintain test coverage

## CAP Best Practices

### Data Modeling

```cds
// ✅ Good: Use aspects for common fields
entity Books : managed {
  key ID : UUID;
  title  : String(111);
  stock  : Integer;
}

// ❌ Bad: Repeat common fields
entity Books {
  key ID : UUID;
  title  : String(111);
  stock  : Integer;
  createdAt : Timestamp;
  createdBy : String;
  modifiedAt : Timestamp;
  modifiedBy : String;
}
```

### Service Implementation

```javascript
// ✅ Good: Validate before operations
this.before('CREATE', 'Books', (req) => {
  if (!req.data.title) {
    req.error(400, 'Title is required');
  }
});

// ❌ Bad: No validation
this.on('CREATE', 'Books', async (req) => {
  return INSERT.into(Books).entries(req.data);
});
```

### Authorization

```cds
// ✅ Good: Explicit authorization
service CatalogService @(requires: 'authenticated-user') {
  entity Books @(restrict: [
    { grant: 'READ', to: 'User' },
    { grant: '*', to: 'Admin' }
  ]) as projection on bookshop.Books;
}

// ❌ Bad: No authorization
service CatalogService {
  entity Books as projection on bookshop.Books;
}
```

## UI5 Best Practices

### View Structure

```xml
<!-- ✅ Good: Semantic controls -->
<ObjectHeader
  title="{title}"
  number="{price}"
  numberUnit="EUR">
  <attributes>
    <ObjectAttribute title="Author" text="{author/name}"/>
  </attributes>
</ObjectHeader>

<!-- ❌ Bad: Generic controls -->
<VBox>
  <Text text="{title}"/>
  <Text text="{price} EUR"/>
  <Text text="{author/name}"/>
</VBox>
```

### Controller Logic

```javascript
// ✅ Good: Separate concerns
onSave: function() {
  this._validateData()
    .then(() => this._saveData())
    .then(() => this._showSuccess())
    .catch((error) => this._handleError(error));
},

// ❌ Bad: Everything in one function
onSave: function() {
  if (this.getView().getModel().getProperty("/title")) {
    this.getView().getModel().submitBatch("updateGroup");
    MessageToast.show("Saved");
  }
}
```

### Data Binding

```xml
<!-- ✅ Good: Use expression binding -->
<ObjectStatus
  text="{= ${stock} > 0 ? 'In Stock' : 'Out of Stock'}"
  state="{= ${stock} > 0 ? 'Success' : 'Error'}"/>

<!-- ❌ Bad: Formatter for simple logic -->
<ObjectStatus
  text="{path: 'stock', formatter: '.formatStockText'}"
  state="{path: 'stock', formatter: '.formatStockState'}"/>
```

## ABAP Best Practices

### Clean ABAP

```abap
" ✅ Good: Descriptive names
DATA(customer_name) = customers[ id = customer_id ]-name.

" ❌ Bad: Cryptic names
DATA(cn) = ct[ i = ci ]-n.
```

### Modern Syntax

```abap
" ✅ Good: Inline declarations
SELECT * FROM books
  INTO TABLE @DATA(lt_books)
  WHERE stock > 0.

" ❌ Bad: Separate declarations
DATA: lt_books TYPE TABLE OF books.
SELECT * FROM books
  INTO TABLE lt_books
  WHERE stock > 0.
```

### Error Handling

```abap
" ✅ Good: Structured error handling
TRY.
    DATA(book) = books[ id = book_id ].
  CATCH cx_sy_itab_line_not_found.
    RAISE EXCEPTION TYPE zcx_book_not_found.
ENDTRY.

" ❌ Bad: No error handling
DATA(book) = books[ id = book_id ].
```

## BTP Best Practices

### Service Configuration

```yaml
# ✅ Good: Explicit resource configuration
resources:
  - name: bookshop-db
    type: com.sap.xs.hdi-container
    parameters:
      service: hana
      service-plan: hdi-shared
      config:
        schema: BOOKSHOP

# ❌ Bad: Minimal configuration
resources:
  - name: bookshop-db
    type: com.sap.xs.hdi-container
```

### Environment Variables

```javascript
// ✅ Good: Use environment variables
const port = process.env.PORT || 4004;
const dbUrl = process.env.DATABASE_URL;

// ❌ Bad: Hardcoded values
const port = 4004;
const dbUrl = "localhost:5432/bookshop";
```

## Performance Best Practices

### Database Queries

```javascript
// ✅ Good: Single query with expand
const books = await SELECT.from(Books).columns(b => {
  b('*'),
  b.author(a => a('*'))
});

// ❌ Bad: N+1 queries
const books = await SELECT.from(Books);
for (const book of books) {
  book.author = await SELECT.one.from(Authors)
    .where({ ID: book.author_ID });
}
```

### OData Queries

```javascript
// ✅ Good: Selective fields
GET /Books?$select=ID,title,price&$expand=author($select=name)

// ❌ Bad: All fields
GET /Books?$expand=author
```

### Caching

```javascript
// ✅ Good: Cache frequently accessed data
const cache = new Map();

this.on('READ', 'Authors', async (req, next) => {
  const key = JSON.stringify(req.query);
  if (cache.has(key)) return cache.get(key);
  
  const result = await next();
  cache.set(key, result);
  return result;
});

// ❌ Bad: No caching
this.on('READ', 'Authors', async (req, next) => {
  return next();
});
```

## Security Best Practices

### Input Validation

```javascript
// ✅ Good: Validate all inputs
this.before('CREATE', 'Books', (req) => {
  const { title, price, stock } = req.data;
  
  if (!title || title.length > 111) {
    req.error(400, 'Invalid title');
  }
  if (price < 0 || price > 9999.99) {
    req.error(400, 'Invalid price');
  }
  if (stock < 0) {
    req.error(400, 'Invalid stock');
  }
});

// ❌ Bad: No validation
this.on('CREATE', 'Books', async (req) => {
  return INSERT.into(Books).entries(req.data);
});
```

### SQL Injection Prevention

```javascript
// ✅ Good: Parameterized queries
const books = await SELECT.from(Books)
  .where({ title: { like: `%${searchTerm}%` } });

// ❌ Bad: String concatenation
const books = await cds.run(
  `SELECT * FROM Books WHERE title LIKE '%${searchTerm}%'`
);
```

### Authentication

```javascript
// ✅ Good: Check authentication
this.before('*', (req) => {
  if (!req.user.is('authenticated-user')) {
    req.reject(401, 'Unauthorized');
  }
});

// ❌ Bad: No authentication check
this.on('READ', 'Books', async (req) => {
  return SELECT.from(Books);
});
```

## Testing Best Practices

### Unit Tests

```javascript
// ✅ Good: Test edge cases
describe('Book validation', () => {
  it('should reject negative stock', async () => {
    await expect(
      srv.create('Books', { title: 'Test', stock: -1 })
    ).to.be.rejected;
  });
  
  it('should reject empty title', async () => {
    await expect(
      srv.create('Books', { title: '', stock: 10 })
    ).to.be.rejected;
  });
});

// ❌ Bad: Only happy path
describe('Book creation', () => {
  it('should create a book', async () => {
    const book = await srv.create('Books', {
      title: 'Test',
      stock: 10
    });
    expect(book).to.exist;
  });
});
```

### Integration Tests

```javascript
// ✅ Good: Test complete flows
describe('Order flow', () => {
  it('should process order and update stock', async () => {
    // Create book
    const book = await POST('/Books', { title: 'Test', stock: 10 });
    
    // Submit order
    const order = await POST('/submitOrder', {
      book: book.ID,
      quantity: 5
    });
    
    // Verify stock updated
    const updated = await GET(`/Books(${book.ID})`);
    expect(updated.stock).to.equal(5);
  });
});

// ❌ Bad: Test only one operation
describe('Order', () => {
  it('should submit order', async () => {
    const order = await POST('/submitOrder', {
      book: 'test-id',
      quantity: 5
    });
    expect(order).to.exist;
  });
});
```

## Documentation Best Practices

### Code Comments

```javascript
// ✅ Good: Explain why, not what
// Calculate discount based on customer loyalty tier
// Premium customers get 20%, regular customers get 10%
const discount = customer.tier === 'premium' ? 0.20 : 0.10;

// ❌ Bad: State the obvious
// Set discount variable
const discount = customer.tier === 'premium' ? 0.20 : 0.10;
```

### API Documentation

```cds
// ✅ Good: Document service and actions
/**
 * Catalog service for managing books and authors
 * @requires authenticated-user
 */
service CatalogService {
  
  /**
   * Submit an order for a book
   * @param book - Book ID
   * @param quantity - Number of books to order
   * @returns Updated stock and confirmation message
   */
  action submitOrder(book: UUID, quantity: Integer) returns {
    stock: Integer;
    message: String;
  };
}

// ❌ Bad: No documentation
service CatalogService {
  action submitOrder(book: UUID, quantity: Integer) returns {
    stock: Integer;
    message: String;
  };
}
```

## Deployment Best Practices

### Version Control

```bash
# ✅ Good: Semantic versioning
git tag -a v1.2.3 -m "Release version 1.2.3"

# ❌ Bad: No versioning
git commit -m "updates"
```

### Environment Configuration

```json
// ✅ Good: Environment-specific config
{
  "production": {
    "db": {
      "kind": "hana"
    }
  },
  "development": {
    "db": {
      "kind": "sqlite"
    }
  }
}

// ❌ Bad: Single configuration
{
  "db": {
    "kind": "sqlite"
  }
}
```

## Summary

Following these best practices ensures:
- ✅ Secure applications
- ✅ High performance
- ✅ Maintainable code
- ✅ Production-ready quality
- ✅ SAP standard compliance
