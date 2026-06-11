# 📘 Pet API Documentation

> **Base URL:** `http://localhost:8080/api/v1`  
> **Authentication:** Bearer Token — thêm header `Authorization: Bearer <jwt_token>` cho các endpoint yêu cầu xác thực.

---

## Mục lục
1. [module-auth](#module-auth)
2. [module-user](#module-user)
3. [module-product](#module-product)
4. [product-review](#product-review)
5. [module-product-images](#module-product-images)
6. [module-inventory](#module-inventory)
7. [module-cart-item](#module-cart-item)
8. [module-cart](#module-cart)
9. [module-shipping-address](#module-shipping-address)
10. [module-order](#module-order)
11. [module-pet](#module-pet)
12. [module-category](#module-category)

---

## module-auth

### 1. Register
- **Method:** `POST`
- **Endpoint:** `/auth/register`
- **Auth:** Không cần
- **Request Body (JSON):**
```json
{
  "username": "string",
  "email": "string",
  "password": "string",
  "fullName": "string"
}
```
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Register successfully",
  "data": { ... }
}
```

---

### 2. Login
- **Method:** `POST`
- **Endpoint:** `/auth/login`
- **Auth:** Không cần
- **Request Body (JSON):**
```json
{
  "email": "string",
  "password": "string"
}
```
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Login successfully",
  "data": {
    "jwt_token": "string"
  }
}
```

---

### 3. Logout
- **Method:** `POST`
- **Endpoint:** `/auth/logout`
- **Auth:** Bearer Token
- **Request Body:** Không có
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Logout successfully"
}
```

---

### 4. Verify Email (Forgot Password)
- **Method:** `POST`
- **Endpoint:** `/forget-password/email-verification/{email}`
- **Auth:** Không cần
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| email | string | Email cần xác minh |

- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "OTP sent to email"
}
```

---

### 5. Verify OTP
- **Method:** `POST`
- **Endpoint:** `/forget-password/otp-verification`
- **Auth:** Không cần
- **Request Body (JSON):**
```json
{
  "email": "string",
  "otp": "string"
}
```
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "OTP verified successfully"
}
```

---

### 6. Change Password
- **Method:** `POST`
- **Endpoint:** `/forget-password/password-update/{email}`
- **Auth:** Không cần
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| email | string | Email tài khoản |

- **Request Body (JSON):**
```json
{
  "newPassword": "string",
  "confirmPassword": "string"
}
```
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Password updated successfully"
}
```

---

## module-user

### 1. Get User By ID
- **Method:** `GET`
- **Endpoint:** `/user/{id}`
- **Auth:** Bearer Token
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | UUID string | ID người dùng |

- **Response 200:**
```json
{
  "statusCode": 200,
  "data": {
    "id": "uuid",
    "email": "string",
    "fullName": "string",
    "role": { "name": "ROLE_USER" }
  }
}
```

---

### 2. Create User
- **Method:** `POST`
- **Endpoint:** `/user`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "email": "string",
  "password": "string",
  "fullName": "string",
  "role": "ROLE_USER"
}
```

---

### 3. Get All Users
- **Method:** `GET`
- **Endpoint:** `/user`
- **Auth:** Bearer Token (Admin)
- **Query Params:**

| Param | Type | Mô tả | Ví dụ |
|-------|------|--------|-------|
| page | int | Trang hiện tại | 0 |
| size | int | Số phần tử mỗi trang | 5 |
| sort | string | Sắp xếp | id,desc |
| filter | string | Lọc dữ liệu | role.name:ROLE_ADMIN |

- **Response 200:**
```json
{
  "statusCode": 200,
  "data": {
    "content": [ { ... } ],
    "totalElements": 10,
    "totalPages": 2
  }
}
```

---

### 4. Update User
- **Method:** `PUT`
- **Endpoint:** `/user`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "id": "uuid",
  "fullName": "string",
  "phone": "string",
  "avatar": "string"
}
```

---

### 5. Delete User
- **Method:** `DELETE`
- **Endpoint:** `/user/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | UUID string | ID người dùng cần xóa |

---

### 6. Change User Status
- **Method:** `PATCH`
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token (Admin)

---

## module-product

### 1. Get A Product
- **Method:** `GET`
- **Endpoint:** `/product/{id}`
- **Auth:** Không cần
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID sản phẩm |

- **Response 200:**
```json
{
  "statusCode": 200,
  "data": {
    "id": 6,
    "name": "string",
    "price": 0,
    "description": "string",
    "images": [ { "id": 1, "url": "string", "isMain": true } ],
    "category": { "id": 1, "name": "string" }
  }
}
```

---

### 2. Get All Products
- **Method:** `GET`
- **Endpoint:** `/product`
- **Auth:** Không cần
- **Query Params:**

| Param | Type | Mô tả | Ví dụ |
|-------|------|--------|-------|
| page | int | Trang | 1 |
| size | int | Số phần tử | 4 |
| sort | string | Sắp xếp | id,desc |
| filter | string | Lọc | price < 700000 |

---

### 3. Create Product
- **Method:** `POST`
- **Endpoint:** `/product`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "name": "string",
  "price": 0,
  "description": "string",
  "categoryId": 1,
  "stock": 0
}
```

---

### 4. Update A Product
- **Method:** `PUT`
- **Endpoint:** `/product`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "id": 1,
  "name": "string",
  "price": 0,
  "description": "string",
  "categoryId": 1
}
```

---

### 5. Delete A Product
- **Method:** `DELETE`
- **Endpoint:** `/product/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID sản phẩm cần xóa |

---

## product-review

### 1. Get Reviews By Product ID
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Không cần

---

### 2. Create Review
- **Method:** `POST`
- **Endpoint:** `/product-reviews`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "rating": 5,
  "comment": "string"
}
```

---

### 3. Update Review
- **Method:** `PUT`
- **Endpoint:** `/product-reviews`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "id": 1,
  "rating": 4,
  "comment": "string"
}
```

---

### 4. Delete Review
- **Method:** `GET` ⚠️ (URL chưa xác định)
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token

---

### 5. Get All Reviews (Admin)
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token (Admin)

---

## module-product-images

### 1. Add Images
- **Method:** `POST`
- **Endpoint:** `/product-images`
- **Auth:** Bearer Token (Admin)
- **Request Body (form-data):**

| Key | Type | Mô tả |
|-----|------|--------|
| productId | text | ID sản phẩm |
| images | file | File ảnh (có thể nhiều file) |

- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Images uploaded successfully",
  "data": [ { "id": 1, "url": "https://res.cloudinary.com/..." } ]
}
```

---

### 2. Delete Image
- **Method:** `DELETE`
- **Endpoint:** `/product-images/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID ảnh cần xóa |

---

### 3. Set Main Image
- **Method:** ⚠️ Chưa xác định
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token (Admin)

---

## module-inventory

### 1. Import Product
- **Method:** `POST`
- **Endpoint:** `/inventory/import`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "quantity": 100,
  "note": "string"
}
```

---

### 2. Export Product
- **Method:** `POST`
- **Endpoint:** `/inventory/export`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "quantity": 10,
  "note": "string"
}
```

---

### 3. Adjust Product
- **Method:** `POST`
- **Endpoint:** `/inventory/adjust`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "quantity": 50,
  "note": "Điều chỉnh tồn kho"
}
```

---

### 4. Get Inventory By Product ID
- **Method:** `GET`
- **Endpoint:** `/inventory/{productId}`
- **Auth:** Bearer Token
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| productId | long | ID sản phẩm |

- **Response 200:**
```json
{
  "statusCode": 200,
  "data": {
    "productId": 6,
    "quantity": 100
  }
}
```

---

### 5. Get Inventory Transaction History
- **Method:** `GET`
- **Endpoint:** `/inventory/transaction-history`
- **Auth:** Bearer Token (Admin)
- **Query Params:**

| Param | Type | Mô tả | Ví dụ |
|-------|------|--------|-------|
| page | int | Trang | 0 |
| size | int | Số phần tử | 5 |
| filter | string | Lọc theo loại | type: IMPORT |

---

## module-cart-item

### 1. Add Cart Item
- **Method:** `POST`
- **Endpoint:** `/cartItem`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "quantity": 2
}
```

---

### 2. Update Cart Item
- **Method:** `PUT`
- **Endpoint:** `/cartItem`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "cartItemId": 1,
  "quantity": 3
}
```

---

## module-cart

### 1. Get Cart
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định (dự kiến `/cart`)
- **Auth:** Bearer Token
- **Response 200:**
```json
{
  "statusCode": 200,
  "data": {
    "id": 1,
    "items": [
      {
        "cartItemId": 1,
        "productId": 1,
        "productName": "string",
        "quantity": 2,
        "price": 100000
      }
    ],
    "totalPrice": 200000
  }
}
```

---

### 2. Delete Cart
- **Method:** `DELETE`
- **Endpoint:** `/cart`
- **Auth:** Bearer Token
- **Response 200:**
```json
{
  "statusCode": 200,
  "message": "Cart deleted successfully"
}
```

---

## module-shipping-address

### 1. Create Shipping Address
- **Method:** `POST`
- **Endpoint:** `/shippingAddress`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "fullName": "string",
  "phone": "string",
  "address": "string",
  "city": "string",
  "district": "string",
  "ward": "string"
}
```

---

### 2. Update Shipping Address
- **Method:** `PUT`
- **Endpoint:** `/shippingAddress`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "id": 1,
  "fullName": "string",
  "phone": "string",
  "address": "string",
  "city": "string",
  "district": "string",
  "ward": "string"
}
```

---

### 3. Get All Shipping Addresses
- **Method:** `GET`
- **Endpoint:** `/shippingAddress`
- **Auth:** Bearer Token
- **Response 200:**
```json
{
  "statusCode": 200,
  "data": [
    {
      "id": 1,
      "fullName": "string",
      "phone": "string",
      "address": "string",
      "isDefault": true
    }
  ]
}
```

---

### 4. Set Default Shipping Address
- **Method:** `PATCH`
- **Endpoint:** `/shippingAddress`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "id": 1
}
```

---

## module-order

### 1. Create Order From Cart
- **Method:** `POST`
- **Endpoint:** `/order/from-cart`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "shippingAddressId": 1,
  "paymentMethod": "COD",
  "note": "string"
}
```

---

### 2. Create Order From Buy Now
- **Method:** `POST`
- **Endpoint:** `/order/from-buy-now`
- **Auth:** Bearer Token
- **Request Body (JSON):**
```json
{
  "productId": 1,
  "quantity": 1,
  "shippingAddressId": 1,
  "paymentMethod": "COD",
  "note": "string"
}
```

---

### 3. Get My Orders
- **Method:** `GET`
- **Endpoint:** `/order/my-orders`
- **Auth:** Bearer Token
- **Response 200:**
```json
{
  "statusCode": 200,
  "data": [
    {
      "id": 1,
      "status": "PENDING",
      "totalPrice": 200000,
      "createdAt": "2024-01-01T00:00:00"
    }
  ]
}
```

---

### 4. Cancel Order
- **Method:** ⚠️ Chưa xác định
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token

---

### 5. Get All Orders (Admin)
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token (Admin)

---

### 6. Get Order Status History By Order ID
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định
- **Auth:** Bearer Token

---

### 7. Update Order Status (Admin)
- **Method:** `PATCH`
- **Endpoint:** `/admin/orders/status`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "orderId": 1,
  "status": "CONFIRMED"
}
```

---

## module-pet

### 1. Get A Pet
- **Method:** `GET`
- **Endpoint:** `/product/{id}`
- **Auth:** Không cần
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID thú cưng |

---

### 2. Get All Pets
- **Method:** `GET`
- **Endpoint:** ⚠️ URL chưa xác định (dự kiến `/product` hoặc `/pet`)
- **Auth:** Không cần

---

### 3. Get All My Pets
- **Method:** `GET`
- **Endpoint:** `/product`
- **Auth:** Bearer Token
- **Query Params:**

| Param | Type | Mô tả | Ví dụ |
|-------|------|--------|-------|
| page | int | Trang | 1 |
| size | int | Số phần tử | 4 |
| sort | string | Sắp xếp | id,desc |
| filter | string | Lọc | price < 700000 |

---

### 4. Create A Pet
- **Method:** `POST`
- **Endpoint:** `/product`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "name": "string",
  "price": 0,
  "description": "string",
  "categoryId": 1,
  "breed": "string",
  "age": 0,
  "gender": "MALE"
}
```

---

### 5. Update A Pet
- **Method:** `PUT`
- **Endpoint:** `/product`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "id": 1,
  "name": "string",
  "price": 0,
  "description": "string",
  "categoryId": 1
}
```

---

### 6. Delete A Pet
- **Method:** `DELETE`
- **Endpoint:** `/product/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID thú cưng cần xóa |

---

### 7. Deactivate Pet
- **Method:** `PATCH`
- **Endpoint:** `/pet/deactive/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID thú cưng |

---

### 8. Activate Pet
- **Method:** `PATCH`
- **Endpoint:** `/pet/activate/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID thú cưng |

---

## module-category

### 1. Create Category
- **Method:** `POST`
- **Endpoint:** `/categories`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "name": "string",
  "description": "string"
}
```

---

### 2. Update Category
- **Method:** `PUT`
- **Endpoint:** `/categories`
- **Auth:** Bearer Token (Admin)
- **Request Body (JSON):**
```json
{
  "id": 1,
  "name": "string",
  "description": "string"
}
```

---

### 3. Delete Category
- **Method:** `DELETE`
- **Endpoint:** `/categories/{id}`
- **Auth:** Bearer Token (Admin)
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID danh mục cần xóa |

---

### 4. Get List Category
- **Method:** `GET`
- **Endpoint:** `/categories`
- **Auth:** Không cần
- **Response 200:**
```json
{
  "statusCode": 200,
  "data": [
    { "id": 1, "name": "string", "description": "string" }
  ]
}
```

---

### 5. Get A Category
- **Method:** `GET`
- **Endpoint:** `/categories/{id}` ⚠️ (URL trong Postman đang trỏ sai sang /orders/my-orders)
- **Auth:** Không cần
- **Path Param:**

| Param | Type | Mô tả |
|-------|------|--------|
| id | long | ID danh mục |

---

*Tài liệu được tạo tự động từ Postman Collection: Pet*  
*Base URL: http://localhost:8080/api/v1*  
*⚠️ Các endpoint có dấu cảnh báo cần xác nhận lại URL với backend team.*
