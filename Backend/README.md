# Backend API Documentation

## User Registration Endpoint

`POST /users/register`

## Description

Registers a new user in the system. This endpoint validates the input, hashes the password, creates a new user, and returns an authentication token along with the user data.

## Request Body

The request body must be a JSON object with the following structure:

```json
{
  "fullname": {
    "firstname": "string (min 3 chars, required)",
    "lastname": "string (min 3 chars, required)"
  },
  "email": "string (valid email, required)",
  "password": "string (min 6 chars, required)"
}
```

### Example

```json
{
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

## Responses

### Success

- **Status Code:** `201 Created`
- **Body:**

```json
{
  "token": "jwt_token_string",
  "user": {
    "_id": "user_id",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
    // other user fields
  }
}
```

### Validation Error

- **Status Code:** `400 Bad Request`
- **Body:**

```json
{
  "errors": [
    {
      "msg": "Error message",
      "param": "field",
      "location": "body"
    }
    // ...
  ]
}
```

---

## User Login Endpoint 

`POST /users/login`

## Description

Authenticates a user using their email and password. Returns a JWT token and user data if credentials are valid.

## Request Body

The request body must be a JSON object with the following structure:

```json
{
  "email": "string (valid email, required)",
  "password": "string (min 6 chars, required)"
}
```

### Example

```json
{
  "email": "john.doe@example.com",
  "password": "securePassword123"
}
```

## Responses

### Success

- **Status Code:** `200 OK`
- **Body:**

```json
{
  "token": "jwt_token_string",
  "user": {
    "_id": "user_id",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
    // other user fields
  }
}
```

### Validation Error

- **Status Code:** `400 Bad Request`
- **Body:**

```json
{
  "errors": [
    {
      "msg": "Error message",
      "param": "field",
      "location": "body"
    }
    // ...
  ]
}
```

### Authentication Error

- **Status Code:** `401 Unauthorized`
- **Body:**

```json
{
  "message": "Invalid email or password"
}
```

---

## Get User Profile Endpoint

`GET /users/profile`

### Description

Returns the authenticated user's profile information.  
**Requires authentication** via JWT token (sent as a cookie or in the `Authorization` header).

### Request

- **Headers:**  
  - `Authorization: Bearer <jwt_token>` (if not using cookies)

### Responses

#### Success

- **Status Code:** `200 OK`
- **Body:**

```json
{
  "_id": "user_id",
  "fullname": {
    "firstname": "John",
    "lastname": "Doe"
  },
  "email": "john.doe@example.com"
  // other user fields
}
```

#### Authentication Error

- **Status Code:** `401 Unauthorized`
- **Body:**

```json
{
  "message": "Authentication required"
}
```

---

## User Logout Endpoint

`GET /users/logout`

### Description

Logs out the authenticated user by blacklisting the JWT token and clearing the authentication cookie.  
**Requires authentication** via JWT token (sent as a cookie or in the `Authorization` header).

### Request

- **Headers:**  
  - `Authorization: Bearer <jwt_token>` (if not using cookies)

### Responses

#### Success

- **Status Code:** `200 OK`
- **Body:**

```json
{
  "message": "Logout successful"
}
```

#### Authentication Error

- **Status Code:** `401 Unauthorized`
- **Body:**

```json
{
  "message": "Authentication required"
}
```

---

## Notes

- Both `email` and `password` are required.
- Returns a JWT token on successful authentication.
- If credentials are invalid, a 401 error is returned.
- All endpoints except `/users/register` and `/users/login` require authentication.
- JWT token can be sent as a cookie or in the `Authorization` header.

---

# Captain Endpoints

## Register Captain

`POST /captains/register`

### Description

Registers a new captain with vehicle information. Validates input, hashes the password, creates a new captain, and returns an authentication token with the captain data.

### Request Body

```json
{
  "fullname": {
    "firstname": "string", // required, min 3 chars
    "lastname": "string"   // required, min 3 chars
  },
  "email": "string",        // required, valid email
  "password": "string",     // required, min 6 chars
  "vehicle": {
    "color": "string",      // required, min 3 chars
    "plate": "string",      // required, min 3 chars
    "capacity": 1,          // required, integer >= 1
    "vehicleType": "string" // required, one of: "car", "motorcyle", "auto"
  }
}
```

### Success Response

```json
{
  "token": "jwt_token_string",
  "captain": {
    "_id": "captain_id",
    "fullname": {
      "firstname": "Jane",
      "lastname": "Smith"
    },
    "email": "jane.smith@example.com",
    "vehicle": {
      "color": "Blue",
      "plate": "ABC-1234",
      "capacity": 4,
      "vehicleType": "car"
    }
    // other captain fields
  }
}
```

### Validation Error

```json
{
  "errors": [
    {
      "msg": "Error message", // e.g. "First name is required"
      "param": "field",       // e.g. "fullname.firstname"
      "location": "body"
    }
    // ...
  ]
}
```

### Duplicate Email Error

```json
{
  "message": "Captain already exists"
}
```

---

## Captain Login

`POST /captains/login`

### Description

Authenticates a captain using their email and password. Returns a JWT token and captain data if credentials are valid.

### Request Body

```json
{
  "email": "string",    // required, valid email
  "password": "string"  // required, min 6 chars
}
```

### Success Response

```json
{
  "token": "jwt_token_string",
  "captain": {
    "_id": "captain_id",
    "fullname": {
      "firstname": "Jane",
      "lastname": "Smith"
    },
    "email": "jane.smith@example.com",
    "vehicle": {
      "color": "Blue",
      "plate": "ABC-1234",
      "capacity": 4,
      "vehicleType": "car"
    }
    // other captain fields
  }
}
```

### Validation Error

```json
{
  "errors": [
    {
      "msg": "Error message", // e.g. "Invalid email address"
      "param": "field",
      "location": "body"
    }
    // ...
  ]
}
```

### Authentication Error

```json
{
  "message": "Captain not found" // or "Invalid credentials"
}
```

---

## Get Captain Profile

`GET /captains/profile`

### Description

Returns the authenticated captain's profile information.  
**Requires authentication** via JWT token (sent as a cookie or in the `Authorization` header).

### Success Response

```json
{
  "captain": {
    "_id": "captain_id",
    "fullname": {
      "firstname": "Jane",
      "lastname": "Smith"
    },
    "email": "jane.smith@example.com",
    "vehicle": {
      "color": "Blue",
      "plate": "ABC-1234",
      "capacity": 4,
      "vehicleType": "car"
    }
    // other captain fields
  }
}
```

### Authentication Error

```json
{
  "message": "Unauthorized"
}
```

---

## Captain Logout

`GET /captains/logout`

### Description

Logs out the authenticated captain by blacklisting the JWT token and clearing the authentication cookie.  
**Requires authentication** via JWT token (sent as a cookie or in the `Authorization` header).

### Success Response

```json
{
  "message": "Logged out successfully"
}
```

### Authentication Error

```json
{
  "message": "Unauthorized"
}
```
