# Go E-commerce REST API 🛒

A feature-rich e-commerce backend API built with Go, MongoDB, and Gin framework. This project implements a complete e-commerce solution with user authentication, product management, shopping cart, and order processing.

![Go Version](https://img.shields.io/badge/Go-1.21+-00ADD8?style=for-the-badge&logo=go)
![MongoDB](https://img.shields.io/badge/MongoDB-4EA94B?style=for-the-badge&logo=mongodb&logoColor=white)
![Gin](https://img.shields.io/badge/Gin-00ADD8?style=for-the-badge&logo=go&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-black?style=for-the-badge&logo=JSON%20web%20tokens)


## 🚀 Features

- **User Authentication**: JWT-based secure authentication system
- **User Management**: Signup, Login with encrypted passwords
- **Product Catalog**: Admin can add products, users can view all products
- **Search Functionality**: Search products by name with regex support
- **Shopping Cart**: Add/remove items, view cart with total price calculation
- **Address Management**: Add, edit, and delete delivery addresses (Home/Work)
- **Order Processing**: Cart checkout and instant buy options
- **MongoDB Integration**: NoSQL database for flexible data modeling
- **Docker Support**: Easy deployment with Docker Compose

## 📋 Table of Contents

- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [Environment Variables](#environment-variables)
- [Contributing](#contributing)

## 🛠️ Tech Stack

- **Language**: Go 1.21+
- **Web Framework**: Gin
- **Database**: MongoDB
- **Authentication**: JWT
- **Password Hashing**: bcrypt
- **Containerization**: Docker & Docker Compose

## 📦 Installation

### Prerequisites
- Go 1.21 or higher
- MongoDB
- Docker and Docker Compose (optional)

### Quick Start

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/go-ecommerce-api.git
cd go-ecommerce-api
```

2. **Start MongoDB with Docker**
```bash
docker-compose up -d
```

3. **Run the application**
```bash
go run main.go
```

The server will start on `http://localhost:8000`

## 📚 API Documentation

### Authentication Endpoints

#### 1. User Signup
- **Method**: `POST`
- **URL**: `/users/signup`
- **Description**: Register a new user account

**Request Body:**
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "securepassword",
  "phone": "+1234567890"
}
```

**Response:**
```
"Successfully Signed Up!!"
```

#### 2. User Login
- **Method**: `POST`
- **URL**: `/users/login`
- **Description**: Authenticate user and receive JWT tokens

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "_id": "61614f539f29be942bd9df8e",
  "first_name": "john",
  "last_name": "doe",
  "password": "$2a$14$UIYjkTfnFnhg4qhIfhtYnuK9qsBQifPKgu/WPZAYBaaN17j0eTQZa",
  "email": "john@example.com",
  "phone": "+1234567890",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "created_at": "2023-01-01T08:14:11Z",
  "updated_at": "2023-01-01T08:14:11Z",
  "user_id": "61614f539f29be942bd9df8e",
  "usercart": [],
  "address": [],
  "orders": []
}
```

### Product Management

#### 3. Add Product (Admin Only)
- **Method**: `POST`
- **URL**: `/admin/addproduct`
- **Description**: Add a new product to the catalog
- **Authorization**: Admin token required

**Request Body:**
```json
{
  "product_name": "Alienware x15",
  "price": 2500,
  "rating": 10,
  "image": "alienware.jpg"
}
```

**Response:**
```
"Successfully added our Product Admin!!"
```

#### 4. View All Products
- **Method**: `GET`
- **URL**: `/users/productview`
- **Description**: Get list of all products

**Response:**
```json
[
  {
    "Product_ID": "6153ff8edef2c3c0a02ae39a",
    "product_name": "Alienware x15",
    "price": 2500,
    "rating": 10,
    "image": "alienware.jpg"
  },
  {
    "Product_ID": "616152679f29be942bd9df8f",
    "product_name": "iPhone 13",
    "price": 1700,
    "rating": 9,
    "image": "iphone.jpg"
  }
]
```

#### 5. Search Products
- **Method**: `GET`
- **URL**: `/users/search?name={searchTerm}`
- **Description**: Search products by name using regex
- **Example**: `/users/search?name=alien`

**Response:**
```json
[
  {
    "Product_ID": "6153ff8edef2c3c0a02ae39a",
    "product_name": "Alienware x15",
    "price": 2500,
    "rating": 10,
    "image": "alienware.jpg"
  }
]
```

### Shopping Cart Operations

#### 6. Add to Cart
- **Method**: `GET`
- **URL**: `/addtocart?id={product_id}&userID={user_id}`
- **Description**: Add a product to user's cart
- **Authorization**: User token required

#### 7. Remove from Cart
- **Method**: `GET`
- **URL**: `/removefromcart?id={product_id}&userID={user_id}`
- **Description**: Remove a product from user's cart
- **Authorization**: User token required

#### 8. View Cart
- **Method**: `GET`
- **URL**: `/listcart?id={user_id}`
- **Description**: List all items in user's cart with total price
- **Authorization**: User token required

### Address Management

#### 9. Add Address
- **Method**: `POST`
- **URL**: `/addaddress?id={user_id}`
- **Description**: Add a new address (limited to 2: home and work)
- **Authorization**: User token required

**Request Body:**
```json
{
  "house_name": "Villa 25",
  "street_name": "Main Street",
  "city_name": "New York",
  "pin_code": "10001"
}
```

#### 10. Edit Home Address
- **Method**: `PUT`
- **URL**: `/edithomeaddress?id={user_id}`
- **Description**: Update user's home address
- **Authorization**: User token required

#### 11. Edit Work Address
- **Method**: `PUT`
- **URL**: `/editworkaddress?id={user_id}`
- **Description**: Update user's work address
- **Authorization**: User token required

#### 12. Delete Addresses
- **Method**: `GET`
- **URL**: `/deleteaddresses?id={user_id}`
- **Description**: Delete all user addresses
- **Authorization**: User token required

### Order Processing

#### 13. Cart Checkout
- **Method**: `GET`
- **URL**: `/cartcheckout?id={user_id}`
- **Description**: Place order with all items in cart (clears cart after order)
- **Authorization**: User token required

#### 14. Instant Buy
- **Method**: `GET`
- **URL**: `/instantbuy?userid={user_id}&pid={product_id}`
- **Description**: Buy a single product instantly
- **Authorization**: User token required

## 📂 Project Structure

```
go-ecommerce-api/
├── controllers/           # HTTP request handlers
│   ├── cart.go           # Cart operations
│   ├── controllers.go    # User authentication & products
│   └── address.go        # Address management
├── database/             # Database connection
│   └── databasetup.go    # MongoDB setup
├── middleware/           # Authentication middleware
│   └── middleware.go     # JWT verification
├── models/              # Data models
│   └── models.go        # User, Product, Order schemas
├── routes/              # API routes
│   └── routes.go        # Route definitions
├── tokens/              # JWT token management
│   └── tokengen.go      # Token generation/validation
├── docker-compose.yaml  # Docker configuration
├── go.mod              # Go dependencies
├── go.sum              # Dependency checksums
├── main.go             # Application entry point
└── README.md           # This file
```

## 🔧 Environment Variables

Create a `.env` file in the root directory:

```env
# Server Configuration
PORT=8000

# MongoDB Configuration
MONGODB_URL=mongodb://localhost:27017
DATABASE_NAME=Ecommerce

# JWT Configuration
SECRET_KEY=your-secret-key-here
```

## 🐳 Docker Configuration

The project includes a `docker-compose.yaml` for easy MongoDB setup:

```yaml
version: '3.8'
services:
  mongodb:
    image: mongo:latest
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
    environment:
      - MONGO_INITDB_DATABASE=Ecommerce

volumes:
  mongodb_data:
```

## 🧪 Testing

### Using cURL

**Signup:**
```bash
curl -X POST http://localhost:8000/users/signup \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Test",
    "last_name": "User",
    "email": "test@example.com",
    "password": "testpass123",
    "phone": "+1234567890"
  }'
```

**Login:**
```bash
curl -X POST http://localhost:8000/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "testpass123"
  }'
```

## 🚀 Deployment

### Local Development
```bash
go mod download
go run main.go
```

### Production Build
```bash
go build -o ecommerce-api
./ecommerce-api
```

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request


## 🙏 Acknowledgments

- Gin Web Framework
- MongoDB Go Driver
- JWT-Go library


---
