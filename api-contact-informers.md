# Contact Informers API

Retrieves information about contact informers and their total Assets Under Management (AUM).

---

## Table of Contents

# Contact Informers API

Retrieves information about contact informers and their total Assets Under Management (AUM).

---

## Table of Contents

1. [Endpoint GET](##endpoint-GET-/v2/informers/aum)
2. [Authentication](##authentication)
3. [Query Parameters](##query-parameters)
4. [Request Example GET](##request-example-GET)
5. [Response Fields](##response-fields)
6. [Create Informer](##create-informer)
7. [Endpoint POST](##endpoint-POST-/v2/informers/aum)
8. [Request Example POST](##request-example-POST)
9. [Successful Response](##successful-response)
10. [Error Handling](##error-handling)
11. [Notes](##notes)

---

## Endpoint GET /v2/informers/aum

Returns a list of contact informers and their total Assets Under Management (AUM). The endpoint retrieves portfolio-level information associated with a specific contact.

---

## Authentication

Requests must include an authorization token.
Authorization: Bearer {access_token}

---

## Query Parameters

| Parameter | Type | Required | Description |
|--------|------|------|-------------|
| contactId | string | Yes | Unique identifier of the contact |
| accountFilter | string | No | Optional filter applied to account data |

---

## Request Example GET

```http
GET /v2/informers/aum?contactId=12345 HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
Successful Response
{
  "contactId": "12345",
  "informers": [
    {
      "name": "Primary Portfolio",
      "aum": 1250000,
      "currency": "USD"
    },
    {
      "name": "Investment Fund",
      "aum": 820000,
      "currency": "USD"
    }
  ]
}
```
## Response Fields

| Field | Type | Description |
|------|------|-------------|
| contactId | string | Contact identifier |
| informers | array | List of informer records |
| name | string | Informer name |
| aum | number | Total assets under management |
| currency | string | Currency of AUM value |

---

## Endpoint POST /v2/informers
Creates a new informer associated with a contact.  
This endpoint stores informer details such as the portfolio name, assets under management (AUM), and currency.

---

### Request Body

```json
{
  "contactId": "12345",
  "name": "Retirement Portfolio",
  "aum": 450000,
  "currency": "USD"
}
```
## Request Example POST
The following example demonstrates how to create a new informer associated with a contact.

The request must include an authorization token and a JSON payload containing informer details such as the contact identifier, informer name, and AUM value.

```http
POST /v2/informers HTTP/1.1
Host: api.example.com
Authorization: Bearer {access_token}
Content-Type: application/json
```
### Request Body

```js
POST /v2/informers HTTP/1.1
Host: api.example.com
Authorization: Bearer {access_token}
Content-Type: application/json
```

## Successful Response

```js
{
  "id": "inf_7891",
  "contactId": "12345",
  "name": "Retirement Portfolio",
  "aum": 450000,
  "currency": "USD",
  "createdAt": "2026-03-01T10:12:45Z"
}
```

## Error Handling

The API uses standard HTTP status codes to indicate success or failure.

### 200 OK
The request completed successfully.

Example:

```
{
  "contactId": "12345",
  "informers": [...]
}
```

### 400 Bad Request
The request contains invalid parameters.
Example:
```
{
  "error": "Invalid contactId format"
}
```
### 401 Unauthorized
Authentication failed or token is missing.
Example:
```
{
  "error": "Authentication token is invalid or expired"
}
```
### 403 Forbidden
The user does not have permission to access this resource.
Example:
```
{
  "error": "Access denied for this contact"
}
```
### 404 Not Found
The requested contact or informer does not exist.
Example:

```
{
  "error": "Contact not found"
}
```
### 500 Internal Server Error
An unexpected server error occurred.
Example:
```
{
  "error": "Unexpected server error"
}
```

## Notes
- Access control is validated through the platform authorization layer.
- Requests are filtered using account-level security rules.
- Responses are processed through a result mapping layer before being returned to the client.


---

## Endpoint GET /v2/informers/aum

Returns a list of contact informers and their total Assets Under Management (AUM). The endpoint retrieves portfolio-level information associated with a specific contact.

---

## Authentication

Requests must include an authorization token.
Authorization: Bearer {access_token}

---

## Query Parameters

| Parameter | Type | Required | Description |
|--------|------|------|-------------|
| contactId | string | Yes | Unique identifier of the contact |
| accountFilter | string | No | Optional filter applied to account data |

---

## Request Example GET

```http
GET /v2/informers/aum?contactId=12345 HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
Successful Response
{
  "contactId": "12345",
  "informers": [
    {
      "name": "Primary Portfolio",
      "aum": 1250000,
      "currency": "USD"
    },
    {
      "name": "Investment Fund",
      "aum": 820000,
      "currency": "USD"
    }
  ]
}
```
## Response Fields

| Field | Type | Description |
|------|------|-------------|
| contactId | string | Contact identifier |
| informers | array | List of informer records |
| name | string | Informer name |
| aum | number | Total assets under management |
| currency | string | Currency of AUM value |

---

## Endpoint POST /v2/informers
Creates a new informer associated with a contact.  
This endpoint stores informer details such as the portfolio name, assets under management (AUM), and currency.

---

### Request Body

```json
{
  "contactId": "12345",
  "name": "Retirement Portfolio",
  "aum": 450000,
  "currency": "USD"
}
```
## Request Example POST
The following example demonstrates how to create a new informer associated with a contact.

The request must include an authorization token and a JSON payload containing informer details such as the contact identifier, informer name, and AUM value.

```http
POST /v2/informers HTTP/1.1
Host: api.example.com
Authorization: Bearer {access_token}
Content-Type: application/json
```
### Request Body

```js
POST /v2/informers HTTP/1.1
Host: api.example.com
Authorization: Bearer {access_token}
Content-Type: application/json
```

## Successful Response

```js
{
  "id": "inf_7891",
  "contactId": "12345",
  "name": "Retirement Portfolio",
  "aum": 450000,
  "currency": "USD",
  "createdAt": "2026-03-01T10:12:45Z"
}
```

## Error Handling

The API uses standard HTTP status codes to indicate success or failure.

### 200 OK
The request completed successfully.

Example:

```
{
  "contactId": "12345",
  "informers": [...]
}
```

### 400 Bad Request
The request contains invalid parameters.
Example:
```
{
  "error": "Invalid contactId format"
}
```
### 401 Unauthorized
Authentication failed or token is missing.
Example:
```
{
  "error": "Authentication token is invalid or expired"
}
```
### 403 Forbidden
The user does not have permission to access this resource.
Example:
```
{
  "error": "Access denied for this contact"
}
```
### 404 Not Found
The requested contact or informer does not exist.
Example:

```
{
  "error": "Contact not found"
}
```
### 500 Internal Server Error
An unexpected server error occurred.
Example:
```
{
  "error": "Unexpected server error"
}
```

## Notes
- Access control is validated through the platform authorization layer.
- Requests are filtered using account-level security rules.
- Responses are processed through a result mapping layer before being returned to the client.
