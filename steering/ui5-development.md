# SAPUI5 Development Guide

Comprehensive guide for building SAPUI5 and Fiori applications using the SAP Skills Power.

## Overview

The `sapui5` and `sap-fiori-tools` skills provide expert guidance for:
- SAPUI5 application structure
- Fiori Elements templates
- Custom controls and views
- OData model binding
- Responsive design
- Accessibility compliance

## Application Structure

### Basic UI5 App

```
webapp/
├── manifest.json          # App descriptor
├── Component.js           # Component controller
├── index.html            # Entry point
├── view/
│   ├── App.view.xml      # Main view
│   └── Main.view.xml     # Content view
├── controller/
│   ├── App.controller.js
│   └── Main.controller.js
├── model/
│   └── models.js         # Model initialization
├── css/
│   └── style.css
└── i18n/
    ├── i18n.properties   # Default language
    └── i18n_de.properties # German
```

## Manifest.json (App Descriptor)

```json
{
  "sap.app": {
    "id": "my.bookshop.app",
    "type": "application",
    "title": "{{appTitle}}",
    "description": "{{appDescription}}",
    "applicationVersion": {
      "version": "1.0.0"
    },
    "dataSources": {
      "mainService": {
        "uri": "/catalog/",
        "type": "OData",
        "settings": {
          "odataVersion": "4.0"
        }
      }
    }
  },
  "sap.ui": {
    "technology": "UI5",
    "deviceTypes": {
      "desktop": true,
      "tablet": true,
      "phone": true
    }
  },
  "sap.ui5": {
    "rootView": {
      "viewName": "my.bookshop.app.view.App",
      "type": "XML",
      "id": "app"
    },
    "dependencies": {
      "minUI5Version": "1.120.0",
      "libs": {
        "sap.ui.core": {},
        "sap.m": {},
        "sap.ui.layout": {}
      }
    },
    "models": {
      "i18n": {
        "type": "sap.ui.model.resource.ResourceModel",
        "settings": {
          "bundleName": "my.bookshop.app.i18n.i18n"
        }
      },
      "": {
        "dataSource": "mainService",
        "preload": true,
        "settings": {
          "synchronizationMode": "None",
          "operationMode": "Server",
          "autoExpandSelect": true,
          "earlyRequests": true
        }
      }
    },
    "routing": {
      "config": {
        "routerClass": "sap.m.routing.Router",
        "viewType": "XML",
        "viewPath": "my.bookshop.app.view",
        "controlId": "app",
        "controlAggregation": "pages",
        "async": true
      },
      "routes": [
        {
          "pattern": "",
          "name": "main",
          "target": "main"
        },
        {
          "pattern": "books/{bookId}",
          "name": "detail",
          "target": "detail"
        }
      ],
      "targets": {
        "main": {
          "viewName": "Main",
          "viewLevel": 1
        },
        "detail": {
          "viewName": "Detail",
          "viewLevel": 2
        }
      }
    }
  }
}
```

## Component.js

```javascript
sap.ui.define([
  "sap/ui/core/UIComponent",
  "sap/ui/model/json/JSONModel",
  "my/bookshop/app/model/models"
], function (UIComponent, JSONModel, models) {
  "use strict";

  return UIComponent.extend("my.bookshop.app.Component", {
    metadata: {
      manifest: "json"
    },

    init: function () {
      // Call parent init
      UIComponent.prototype.init.apply(this, arguments);

      // Set device model
      this.setModel(models.createDeviceModel(), "device");

      // Create router
      this.getRouter().initialize();

      // Set app model
      const appModel = new JSONModel({
        busy: false,
        layout: "TwoColumnsMidExpanded"
      });
      this.setModel(appModel, "app");
    }
  });
});
```

## Views and Controllers

### List View (Main.view.xml)

```xml
<mvc:View
  controllerName="my.bookshop.app.controller.Main"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:core="sap.ui.core"
  displayBlock="true">
  
  <Page
    id="page"
    title="{i18n>mainTitle}">
    
    <content>
      <Table
        id="booksTable"
        items="{
          path: '/Books',
          parameters: {
            $expand: 'author'
          }
        }"
        mode="SingleSelectMaster"
        selectionChange=".onSelectionChange">
        
        <columns>
          <Column width="12em">
            <Text text="{i18n>columnTitle}"/>
          </Column>
          <Column minScreenWidth="Tablet" demandPopin="true">
            <Text text="{i18n>columnAuthor}"/>
          </Column>
          <Column minScreenWidth="Tablet" demandPopin="true" hAlign="End">
            <Text text="{i18n>columnStock}"/>
          </Column>
          <Column minScreenWidth="Tablet" demandPopin="true" hAlign="End">
            <Text text="{i18n>columnPrice}"/>
          </Column>
        </columns>
        
        <items>
          <ColumnListItem type="Navigation" press=".onPress">
            <cells>
              <ObjectIdentifier
                title="{title}"
                text="{ID}"/>
              <Text text="{author/name}"/>
              <ObjectNumber
                number="{stock}"
                state="{= ${stock} > 0 ? 'Success' : 'Error'}"/>
              <ObjectNumber
                number="{
                  path: 'price',
                  type: 'sap.ui.model.type.Currency',
                  formatOptions: {
                    showMeasure: false
                  }
                }"
                unit="EUR"/>
            </cells>
          </ColumnListItem>
        </items>
      </Table>
    </content>
    
    <footer>
      <Toolbar>
        <ToolbarSpacer/>
        <Button
          text="{i18n>createButton}"
          press=".onCreate"
          type="Emphasized"/>
      </Toolbar>
    </footer>
  </Page>
</mvc:View>
```

### List Controller (Main.controller.js)

```javascript
sap.ui.define([
  "sap/ui/core/mvc/Controller",
  "sap/ui/model/Filter",
  "sap/ui/model/FilterOperator",
  "sap/m/MessageToast"
], function (Controller, Filter, FilterOperator, MessageToast) {
  "use strict";

  return Controller.extend("my.bookshop.app.controller.Main", {
    
    onInit: function () {
      // Initialize
    },

    onSelectionChange: function (oEvent) {
      const oItem = oEvent.getParameter("listItem");
      const oContext = oItem.getBindingContext();
      const sPath = oContext.getPath();
      const sBookId = oContext.getProperty("ID");
      
      // Navigate to detail
      this.getOwnerComponent().getRouter().navTo("detail", {
        bookId: sBookId
      });
    },

    onPress: function (oEvent) {
      const oContext = oEvent.getSource().getBindingContext();
      const sBookId = oContext.getProperty("ID");
      
      this.getOwnerComponent().getRouter().navTo("detail", {
        bookId: sBookId
      });
    },

    onCreate: function () {
      // Open create dialog
      MessageToast.show("Create new book");
    },

    onSearch: function (oEvent) {
      const sQuery = oEvent.getParameter("query");
      const oTable = this.byId("booksTable");
      const oBinding = oTable.getBinding("items");
      
      if (sQuery) {
        const aFilters = [
          new Filter({
            filters: [
              new Filter("title", FilterOperator.Contains, sQuery),
              new Filter("author/name", FilterOperator.Contains, sQuery)
            ],
            and: false
          })
        ];
        oBinding.filter(aFilters);
      } else {
        oBinding.filter([]);
      }
    }
  });
});
```

### Detail View (Detail.view.xml)

```xml
<mvc:View
  controllerName="my.bookshop.app.controller.Detail"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:f="sap.ui.layout.form">
  
  <Page
    id="page"
    title="{title}"
    showNavButton="true"
    navButtonPress=".onNavBack">
    
    <content>
      <ObjectHeader
        title="{title}"
        number="{
          path: 'price',
          type: 'sap.ui.model.type.Currency',
          formatOptions: {
            showMeasure: false
          }
        }"
        numberUnit="EUR"
        numberState="{= ${stock} > 0 ? 'Success' : 'Error'}">
        
        <attributes>
          <ObjectAttribute title="{i18n>author}" text="{author/name}"/>
          <ObjectAttribute title="{i18n>stock}" text="{stock}"/>
        </attributes>
        
        <statuses>
          <ObjectStatus
            text="{= ${stock} > 0 ? ${i18n>inStock} : ${i18n>outOfStock}}"
            state="{= ${stock} > 0 ? 'Success' : 'Error'}"/>
        </statuses>
      </ObjectHeader>
      
      <f:SimpleForm
        editable="true"
        layout="ResponsiveGridLayout">
        
        <f:content>
          <Label text="{i18n>title}"/>
          <Input value="{title}"/>
          
          <Label text="{i18n>author}"/>
          <Input value="{author/name}"/>
          
          <Label text="{i18n>stock}"/>
          <StepInput value="{stock}" min="0"/>
          
          <Label text="{i18n>price}"/>
          <Input
            value="{
              path: 'price',
              type: 'sap.ui.model.type.Currency'
            }"/>
        </f:content>
      </f:SimpleForm>
    </content>
    
    <footer>
      <Toolbar>
        <ToolbarSpacer/>
        <Button text="{i18n>saveButton}" press=".onSave" type="Emphasized"/>
        <Button text="{i18n>cancelButton}" press=".onCancel"/>
      </Toolbar>
    </footer>
  </Page>
</mvc:View>
```

### Detail Controller (Detail.controller.js)

```javascript
sap.ui.define([
  "sap/ui/core/mvc/Controller",
  "sap/ui/core/routing/History",
  "sap/m/MessageToast",
  "sap/m/MessageBox"
], function (Controller, History, MessageToast, MessageBox) {
  "use strict";

  return Controller.extend("my.bookshop.app.controller.Detail", {
    
    onInit: function () {
      const oRouter = this.getOwnerComponent().getRouter();
      oRouter.getRoute("detail").attachPatternMatched(this._onPatternMatched, this);
    },

    _onPatternMatched: function (oEvent) {
      const sBookId = oEvent.getParameter("arguments").bookId;
      
      this.getView().bindElement({
        path: `/Books(${sBookId})`,
        parameters: {
          $expand: "author"
        },
        events: {
          dataRequested: () => {
            this.getView().setBusy(true);
          },
          dataReceived: () => {
            this.getView().setBusy(false);
          }
        }
      });
    },

    onNavBack: function () {
      const oHistory = History.getInstance();
      const sPreviousHash = oHistory.getPreviousHash();
      
      if (sPreviousHash !== undefined) {
        window.history.go(-1);
      } else {
        this.getOwnerComponent().getRouter().navTo("main", {}, true);
      }
    },

    onSave: function () {
      const oModel = this.getView().getModel();
      const oContext = this.getView().getBindingContext();
      
      if (oModel.hasPendingChanges()) {
        oModel.submitBatch("updateGroup").then(() => {
          MessageToast.show("Book saved successfully");
          this.onNavBack();
        }).catch((oError) => {
          MessageBox.error("Failed to save book: " + oError.message);
        });
      } else {
        this.onNavBack();
      }
    },

    onCancel: function () {
      const oModel = this.getView().getModel();
      
      if (oModel.hasPendingChanges()) {
        MessageBox.confirm("Discard changes?", {
          onClose: (sAction) => {
            if (sAction === MessageBox.Action.OK) {
              oModel.resetChanges();
              this.onNavBack();
            }
          }
        });
      } else {
        this.onNavBack();
      }
    }
  });
});
```

## Fiori Elements

### List Report

```json
// manifest.json
{
  "sap.ui5": {
    "routing": {
      "routes": [
        {
          "pattern": ":?query:",
          "name": "BooksList",
          "target": "BooksList"
        },
        {
          "pattern": "Books({key}):?query:",
          "name": "BooksObjectPage",
          "target": "BooksObjectPage"
        }
      ],
      "targets": {
        "BooksList": {
          "type": "Component",
          "id": "BooksList",
          "name": "sap.fe.templates.ListReport",
          "options": {
            "settings": {
              "entitySet": "Books",
              "variantManagement": "Page",
              "navigation": {
                "Books": {
                  "detail": {
                    "route": "BooksObjectPage"
                  }
                }
              }
            }
          }
        },
        "BooksObjectPage": {
          "type": "Component",
          "id": "BooksObjectPage",
          "name": "sap.fe.templates.ObjectPage",
          "options": {
            "settings": {
              "entitySet": "Books"
            }
          }
        }
      }
    }
  }
}
```

## Responsive Design

```xml
<!-- Use responsive controls -->
<Table
  items="{/Books}"
  growing="true"
  growingThreshold="20">
  
  <columns>
    <!-- Always visible -->
    <Column width="12em">
      <Text text="Title"/>
    </Column>
    
    <!-- Hidden on phone -->
    <Column minScreenWidth="Tablet" demandPopin="true">
      <Text text="Author"/>
    </Column>
    
    <!-- Hidden on phone and tablet -->
    <Column minScreenWidth="Desktop" demandPopin="true">
      <Text text="Description"/>
    </Column>
  </columns>
</Table>

<!-- Responsive padding -->
<Page class="sapUiResponsiveContentPadding">
  <content>
    <!-- Content -->
  </content>
</Page>
```

## Accessibility

```xml
<!-- Add ARIA labels -->
<Button
  text="Save"
  press=".onSave"
  ariaLabelledBy="saveButtonLabel"/>

<InvisibleText id="saveButtonLabel" text="Save the current book"/>

<!-- Use semantic controls -->
<ObjectStatus
  text="In Stock"
  state="Success"
  icon="sap-icon://accept"/>

<!-- Keyboard navigation -->
<List
  items="{/Books}"
  mode="SingleSelectMaster"
  includeItemInSelection="true">
  <!-- Items -->
</List>
```

## Best Practices

1. **Use OData V4 Model**: Better performance and features
2. **Implement Routing**: For navigation and deep linking
3. **Add i18n**: Internationalization from the start
4. **Responsive Design**: Support all device types
5. **Accessibility**: WCAG 2.1 AA compliance
6. **Error Handling**: User-friendly error messages
7. **Performance**: Lazy loading, pagination, caching
8. **Testing**: Unit tests with QUnit, integration tests with OPA5

## Resources

- [SAPUI5 Documentation](https://ui5.sap.com/)
- [Fiori Design Guidelines](https://experience.sap.com/fiori-design/)
- [UI5 Samples](https://ui5.sap.com/#/controls)
- [Fiori Elements](https://ui5.sap.com/#/topic/03265b0408e2432c9571d6b3feb6b1fd)
