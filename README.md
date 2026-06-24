# Project

## Run

-  cd .\inventory\
    - npm install --force
    - npm run start  
    - http://localhost:4200/

- cd .\products\
    - npm install --force
    - npm run dev
    - http://localhost:5173/

## Overview

This project is a simple inventory management system composed of:

- React Frontend → Product CRUD management
- Angular Frontend → Inventory CRUD management
- .NET Web API → Backend services
- SQL Server Database → Data storage
- No authentication or authorization is required.

## Business Rules

- Price is stored as an integer in cents.
    - 1999 = $19.99
    - 500 = $5.00
    - 125050 = $1,250.50
- must be greater than or equal to 0.
- Frontend converts cents to a currency value for display.
- API accepts and returns prices in cents.
- Inventory price can differ from product price if inventory pricing changes independently.

## Models

```c#
public class Product
{
    public int Id { get; set; }

    public string Name { get; set; }

    public string Description { get; set; }

    // Stored in cents
    public int Price { get; set; }
}

public class Inventory
{
    public int InventoryId { get; set; }

    public int ProductId { get; set; }

    public int Quantity { get; set; }

    // Stored in cents
    public int Price { get; set; }
}
```
## Database Design

```sql
CREATE TABLE Product (
    Id INT IDENTITY(1,1) PRIMARY KEY,
    Name NVARCHAR(100) NOT NULL,
    Description NVARCHAR(500) NULL,
    Price INT NOT NULL, -- Stored in cents
    CreatedDate DATETIME DEFAULT GETDATE()
);

CREATE TABLE Inventory (
    InventoryId INT IDENTITY(1,1) PRIMARY KEY,
    ProductId INT NOT NULL,
    Quantity INT NOT NULL,
    Price INT NOT NULL, -- Stored in cents
    LastUpdated DATETIME DEFAULT GETDATE(),

    CONSTRAINT FK_Inventory_Product
        FOREIGN KEY (ProductId)
        REFERENCES Product(Id)
);
```

## Frontend

```js
const displayPrice = (price / 100).toFixed(2);
```

## APIs

### Product Controller

- GET Products/{searchterm} (paginated)

- POST Product

```json
{
  "name": "Laptop",
  "description": "Gaming Laptop",
  "price": 120050
}
```

- PUT PRODUCT

```json
{
  "id": 1,
  "name": "Laptop",
  "description": "Gaming Laptop",
  "price": 120050
}
```

- DELETE PRODUCT/{id}

### Product Inventory

- GET Inventory/{searchterm} (paginated)

- POST Inventory

```json
{
  "productId": 1,
  "quantity": 100,
  "price": 120050
}
```

- PUT Inventory

```json
{
  "id": 1,
  "productId": 1,
  "quantity": 80,
  "price": 120050
}
```

- DELETE Inventory/{id}

# Concepts

## Archicteture

## React

```
ProductPage
    ├── ProductList
    └── ProductForm
```

### State

Data owned and managed by a component

```json
function ProductPage() {
  const [products, setProducts] = useState([]);

  return <ProductList products={products} />;
}
```

### Props

```json
function ProductList({ products }) {
  return (
    <>
      {products.map(product => (
        <div key={product.id}>{product.name}</div>
      ))}
    </>
  );
}
```

### Hook useState

```json
function ProductForm() {
  const [name, setName] = useState("");
  const [description, setDescription] = useState("");
  const [price, setPrice] = useState("");

  return (
    <>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
      />

      <input
        value={description}
        onChange={(e) => setDescription(e.target.value)}
      />

      <input
        value={price}
        onChange={(e) => setPrice(e.target.value)}
      />
    </>
  );
}
```

### Hook useEffect

Load Products on Page Load

```json
const [products, setProducts] = useState([]);

useEffect(() => {
  fetchProducts();
}, []);

const fetchProducts = async () => {
  const response = await fetch("/api/products");
  const data = await response.json();

  setProducts(data);
};
```

### Hook useMemo

- TO DO

### Hook useCallback

- TO DO
### Hook useRef

- TO DO

## Angular

## DotNet

## Database