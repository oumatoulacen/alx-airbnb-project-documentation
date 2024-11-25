# Requirements Documentation

This document outlines the technical and functional requirements for the backend features of the Airbnb Clone project.

---

## 1. User Authentication

### **Functional Requirements**
- Allow users to register an account.
- Enable users to log in and out securely.
- Implement password recovery.

### **Technical Requirements**
- **API Endpoints**:
  - **POST** `/api/auth/register`
    - **Input**:  
      ```json
      {
        "username": "string",
        "email": "string",
        "password": "string"
      }
      ```
    - **Output**:  
      ```json
      {
        "message": "User registered successfully",
        "userId": "UUID"
      }
      ```
    - **Validation Rules**:
      - `username`: Required, string, max 50 chars.
      - `email`: Required, valid email format.
      - `password`: Required, min 8 chars, must include at least one special character.

  - **POST** `/api/auth/login`
    - **Input**:  
      ```json
      {
        "email": "string",
        "password": "string"
      }
      ```
    - **Output**:  
      ```json
      {
        "message": "Login successful",
        "token": "JWT"
      }
      ```

  - **POST** `/api/auth/recover-password`
    - **Input**:  
      ```json
      {
        "email": "string"
      }
      ```
    - **Output**:  
      ```json
      {
        "message": "Recovery email sent"
      }
      ```

### **Performance Criteria**
- Registration and login operations must complete in under 300ms.
- Tokens should expire after 1 hour for security.

---

## 2. Property Management

### **Functional Requirements**
- Allow users to add, edit, and delete properties.
- Enable searching and filtering properties by location, price, and availability.

### **Technical Requirements**
- **API Endpoints**:
  - **POST** `/api/properties`
    - **Input**:  
      ```json
      {
        "title": "string",
        "description": "string",
        "location": "string",
        "price_per_night": "float",
        "availability": "boolean"
      }
      ```
    - **Output**:  
      ```json
      {
        "message": "Property added successfully",
        "propertyId": "UUID"
      }
      ```

  - **GET** `/api/properties`
    - **Input**: Optional query parameters:
      - `location`: string
      - `minPrice`: float
      - `maxPrice`: float
    - **Output**:  
      ```json
      [
        {
          "propertyId": "UUID",
          "title": "string",
          "location": "string",
          "price_per_night": "float"
        }
      ]
      ```

  - **DELETE** `/api/properties/:id`
    - **Output**:  
      ```json
      {
        "message": "Property deleted successfully"
      }
      ```

### **Validation Rules**
- `title`: Required, max 100 chars.
- `price_per_night`: Required, must be a positive float.
- `location`: Required.

### **Performance Criteria**
- Property search operations must return results in under 500ms for up to 100,000 records.

---

## 3. Booking System

### **Functional Requirements**
- Enable users to book available properties.
- Store booking details like dates and user information.

### **Technical Requirements**
- **API Endpoints**:
  - **POST** `/api/bookings`
    - **Input**:  
      ```json
      {
        "propertyId": "UUID",
        "userId": "UUID",
        "checkInDate": "date",
        "checkOutDate": "date"
      }
      ```
    - **Output**:  
      ```json
      {
        "message": "Booking confirmed",
        "bookingId": "UUID"
      }
      ```

  - **GET** `/api/bookings/:id`
    - **Output**:  
      ```json
      {
        "bookingId": "UUID",
        "propertyId": "UUID",
        "checkInDate": "date",
        "checkOutDate": "date",
        "userId": "UUID"
      }
      ```

### **Validation Rules**
- `checkInDate`: Must be a valid date in the future.
- `checkOutDate`: Must be later than `checkInDate`.

### **Performance Criteria**
- Booking confirmation must complete in under 300ms.
- Support up to 1,000 simultaneous booking requests.
