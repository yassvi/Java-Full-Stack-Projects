# Car Maintenance Prediction System - API Documentation

Complete REST API documentation for the Car Maintenance Prediction System backend.

## Base URL
```
http://localhost:8080/api
```

## Authentication

All protected endpoints require JWT token in Authorization header:
```
Authorization: Bearer {token}
```

---

## Authentication Endpoints

### Register User
Register a new user account.

**Endpoint:** `POST /auth/register`

**Request Body:**
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Success Response (201 Created):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "john@example.com",
  "name": "John Doe"
}
```

**Error Responses:**
- `400 Bad Request`: Invalid input (email exists, weak password, etc.)
- `500 Internal Server Error`: Server error

**Example:**
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123"
  }'
```

---

### Login User
Authenticate user and get JWT token.

**Endpoint:** `POST /auth/login`

**Request Body:**
```json
{
  "email": "john@example.com",
  "password": "password123"
}
```

**Success Response (200 OK):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9...",
  "type": "Bearer",
  "userId": 1,
  "email": "john@example.com",
  "name": "John Doe"
}
```

**Error Responses:**
- `400 Bad Request`: Missing fields
- `401 Unauthorized`: Invalid credentials
- `500 Internal Server Error`: Server error

**Example:**
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "john@example.com",
    "password": "password123"
  }'
```

---

## Vehicle Endpoints

### Create Vehicle
Add a new vehicle to user's garage.

**Endpoint:** `POST /api/vehicles`

**Authentication:** Required

**Request Body:**
```json
{
  "name": "My Honda",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 12000
}
```

**Success Response (201 Created):**
```json
{
  "id": 1,
  "name": "My Honda",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 12000
}
```

**Error Responses:**
- `400 Bad Request`: Invalid input
- `401 Unauthorized`: No token
- `500 Internal Server Error`: Server error

---

### Get All Vehicles
Get all vehicles for current user.

**Endpoint:** `GET /api/vehicles`

**Authentication:** Required

**Success Response (200 OK):**
```json
[
  {
    "id": 1,
    "name": "My Honda",
    "model": "Honda Civic 2020",
    "fuelType": "PETROL",
    "purchaseDate": "2020-05-15",
    "kmDriven": 12000
  },
  {
    "id": 2,
    "name": "My Toyota",
    "model": "Toyota Corolla 2019",
    "fuelType": "DIESEL",
    "purchaseDate": "2019-03-10",
    "kmDriven": 35000
  }
]
```

**Example:**
```bash
curl -X GET http://localhost:8080/api/vehicles \
  -H "Authorization: Bearer {token}"
```

---

### Get Vehicle by ID
Get specific vehicle details.

**Endpoint:** `GET /api/vehicles/{vehicleId}`

**Authentication:** Required

**Parameters:**
- `vehicleId` (path): Vehicle ID

**Success Response (200 OK):**
```json
{
  "id": 1,
  "name": "My Honda",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 12000
}
```

**Error Responses:**
- `404 Not Found`: Vehicle not found
- `401 Unauthorized`: No token

---

### Update Vehicle
Update vehicle information.

**Endpoint:** `PUT /api/vehicles/{vehicleId}`

**Authentication:** Required

**Request Body:**
```json
{
  "name": "My Honda Updated",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 15000
}
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "name": "My Honda Updated",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 15000
}
```

---

### Delete Vehicle
Delete a vehicle.

**Endpoint:** `DELETE /api/vehicles/{vehicleId}`

**Authentication:** Required

**Success Response (204 No Content)**

**Error Responses:**
- `404 Not Found`: Vehicle not found
- `401 Unauthorized`: No token

---

## Service Record Endpoints

### Add Service Record
Add a service record for a vehicle.

**Endpoint:** `POST /api/services/{vehicleId}`

**Authentication:** Required

**Request Body:**
```json
{
  "serviceDate": "2024-03-01",
  "kmAtService": 10000,
  "serviceType": "Oil Change",
  "notes": "Regular maintenance"
}
```

**Success Response (201 Created):**
```json
{
  "id": 1,
  "serviceDate": "2024-03-01",
  "kmAtService": 10000,
  "serviceType": "Oil Change",
  "notes": "Regular maintenance",
  "vehicleId": 1
}
```

---

### Get Service Records by Vehicle
Get all service records for a vehicle.

**Endpoint:** `GET /api/services/vehicle/{vehicleId}`

**Authentication:** Required

**Success Response (200 OK):**
```json
[
  {
    "id": 1,
    "serviceDate": "2024-03-01",
    "kmAtService": 10000,
    "serviceType": "Oil Change",
    "notes": "Regular maintenance",
    "vehicleId": 1
  },
  {
    "id": 2,
    "serviceDate": "2023-09-15",
    "kmAtService": 5000,
    "serviceType": "General Service",
    "notes": "Full service",
    "vehicleId": 1
  }
]
```

---

### Get Service Record by ID
Get specific service record.

**Endpoint:** `GET /api/services/{serviceId}`

**Authentication:** Required

**Success Response (200 OK):**
```json
{
  "id": 1,
  "serviceDate": "2024-03-01",
  "kmAtService": 10000,
  "serviceType": "Oil Change",
  "notes": "Regular maintenance",
  "vehicleId": 1
}
```

---

### Update Service Record
Update service record details.

**Endpoint:** `PUT /api/services/{serviceId}`

**Authentication:** Required

**Request Body:**
```json
{
  "serviceDate": "2024-03-01",
  "kmAtService": 10000,
  "serviceType": "Oil Change and Filter",
  "notes": "Updated notes"
}
```

**Success Response (200 OK):**
```json
{
  "id": 1,
  "serviceDate": "2024-03-01",
  "kmAtService": 10000,
  "serviceType": "Oil Change and Filter",
  "notes": "Updated notes",
  "vehicleId": 1
}
```

---

### Delete Service Record
Delete a service record.

**Endpoint:** `DELETE /api/services/{serviceId}`

**Authentication:** Required

**Success Response (204 No Content)**

---

## Maintenance Prediction Endpoints

### Get Maintenance Prediction
Get maintenance prediction for a vehicle.

**Endpoint:** `GET /api/prediction/{vehicleId}`

**Authentication:** Required

**Success Response (200 OK):**
```json
{
  "vehicleId": 1,
  "vehicleName": "My Honda",
  "riskLevel": "LOW",
  "recommendation": "Your vehicle is in good maintenance status.",
  "kmDrivenSinceLastService": 2000,
  "daysSinceLastService": 30,
  "lastServiceDate": "2024-03-01",
  "lastServiceKm": 10000,
  "vehicleAgeInYears": 4,
  "isServiceRequired": false
}
```

**Risk Level Values:**
- `LOW` - Vehicle is well maintained
- `MEDIUM` - Service approaching
- `HIGH` - Immediate service required

**Response Fields:**
| Field | Type | Description |
|-------|------|-------------|
| vehicleId | Long | Vehicle ID |
| vehicleName | String | Vehicle name |
| riskLevel | Enum | LOW/MEDIUM/HIGH |
| recommendation | String | Maintenance recommendation |
| kmDrivenSinceLastService | Long | KM since last service |
| daysSinceLastService | Integer | Days since last service |
| lastServiceDate | Date | Date of last service |
| lastServiceKm | Long | KM at last service |
| vehicleAgeInYears | Integer | Age of vehicle in years |
| isServiceRequired | Boolean | Whether service is required |

**Example:**
```bash
curl -X GET http://localhost:8080/api/prediction/1 \
  -H "Authorization: Bearer {token}"
```

---

## Dashboard Endpoints

### Get Dashboard Summary
Get dashboard summary data.

**Endpoint:** `GET /api/dashboard/summary`

**Authentication:** Required

**Success Response (200 OK):**
```json
{
  "totalVehicles": 2,
  "vehicles": [
    {
      "id": 1,
      "name": "My Honda",
      "model": "Honda Civic 2020",
      "fuelType": "PETROL",
      "purchaseDate": "2020-05-15",
      "kmDriven": 12000
    }
  ],
  "upcomingServices": 1
}
```

---

## Error Handling

### Error Response Format
```json
{
  "message": "Error description"
}
```

### Common Error Codes

| Code | Message | Reason |
|------|---------|--------|
| 400 | Bad Request | Invalid input data |
| 401 | Unauthorized | Missing or invalid token |
| 403 | Forbidden | User doesn't have permission |
| 404 | Not Found | Resource not found |
| 500 | Internal Server Error | Server error |

### Example Error Response
```json
{
  "message": "Email already exists"
}
```

---

## Data Validation Rules

### User Registration
- Name: 2-100 characters
- Email: Valid email format
- Password: Minimum 6 characters

### Vehicle
- Name: Required, non-empty
- Model: Required, non-empty
- Fuel Type: PETROL, DIESEL, or ELECTRIC
- Purchase Date: Valid date, not in future
- KM Driven: Non-negative number

### Service Record
- Service Date: Valid date, not in future
- KM at Service: Non-negative number
- Service Type: Required, non-empty
- Notes: Optional

---

## Rate Limiting

Currently no rate limiting implemented. It's recommended to add rate limiting for production:
- 100 requests per minute per IP for public endpoints
- 500 requests per minute per user for authenticated endpoints

---

## CORS Configuration

Currently allows requests from:
- `http://localhost:5173` (React dev server)
- `http://localhost:3000` (Alternative frontend)

Methods allowed:
- GET, POST, PUT, DELETE, OPTIONS

---

## Example Workflow

### 1. Register
```bash
POST /api/auth/register
Body: {name, email, password}
Response: {token, userId, email, name}
```

### 2. Add Vehicle
```bash
POST /api/vehicles
Headers: Authorization: Bearer {token}
Body: {name, model, fuelType, purchaseDate, kmDriven}
Response: {id, name, model, ...}
```

### 3. Add Service
```bash
POST /api/services/{vehicleId}
Headers: Authorization: Bearer {token}
Body: {serviceDate, kmAtService, serviceType, notes}
Response: {id, serviceDate, ...}
```

### 4. Get Prediction
```bash
GET /api/prediction/{vehicleId}
Headers: Authorization: Bearer {token}
Response: {riskLevel, recommendation, ...}
```

---

## Testing with cURL

### Register
```bash
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@test.com","password":"pass123"}'
```

### Login
```bash
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@test.com","password":"pass123"}'
```

### Add Vehicle (replace TOKEN)
```bash
curl -X POST http://localhost:8080/api/vehicles \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer TOKEN" \
  -d '{"name":"My Car","model":"Car Model","fuelType":"PETROL","purchaseDate":"2020-01-01","kmDriven":5000}'
```

---

## Testing with Postman

1. Import collection from API documentation
2. Create environment variables:
   - `base_url`: http://localhost:8080/api
   - `token`: (obtained from login)
3. Run requests in order
4. Use test scripts for validation

---

## Future API Enhancements

- [ ] Pagination for list endpoints
- [ ] Filtering and search
- [ ] Sorting options
- [ ] Batch operations
- [ ] Webhook support
- [ ] GraphQL endpoint
- [ ] API versioning
- [ ] Rate limiting
- [ ] Request logging
- [ ] Analytics endpoints

---

## Support

For issues with API:
1. Check request format
2. Verify authentication token
3. Check error message
4. Review backend logs
5. Verify database connectivity
