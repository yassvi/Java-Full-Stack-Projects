# Car Maintenance Prediction System - Complete Setup Guide

This guide provides step-by-step instructions to set up and run the entire Car Maintenance Prediction System from scratch.

## Prerequisites Installation

### 1. Install MySQL 8.0+
- Download from: https://dev.mysql.com/downloads/mysql/
- Follow installation wizard
- Note your MySQL password

### 2. Install Java 17+
- Download from: https://www.oracle.com/java/technologies/downloads/
- Or use: https://adoptopenjdk.net/ (free option)
- Verify installation: `java -version`

### 3. Install Node.js 16+ and npm
- Download from: https://nodejs.org/
- Verify installation: 
  ```bash
  node --version
  npm --version
  ```

### 4. Install Maven 3.6+
- Download from: https://maven.apache.org/download.cgi
- OR use: `choco install maven` (Windows)
- Verify installation: `mvn --version`

---

## Backend Setup (Spring Boot)

### Step 1: Create Database

**Using MySQL Command Line:**
```bash
# Open MySQL Command Line Client
# Or connect via MySQL Workbench

mysql -u root -p
# Enter your password

# Execute:
CREATE DATABASE IF NOT EXISTS car_maintenance_db;
USE car_maintenance_db;

-- Create Users Table
CREATE TABLE IF NOT EXISTS users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Create Vehicles Table
CREATE TABLE IF NOT EXISTS vehicles (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id BIGINT NOT NULL,
    name VARCHAR(100) NOT NULL,
    model VARCHAR(100) NOT NULL,
    fuel_type VARCHAR(20) NOT NULL,
    purchase_date DATE NOT NULL,
    km_driven BIGINT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Create Service Records Table
CREATE TABLE IF NOT EXISTS service_records (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    vehicle_id BIGINT NOT NULL,
    service_date DATE NOT NULL,
    km_at_service BIGINT NOT NULL,
    service_type VARCHAR(100) NOT NULL,
    notes LONGTEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (vehicle_id) REFERENCES vehicles(id) ON DELETE CASCADE
);

-- Create Indexes
CREATE INDEX idx_user_email ON users(email);
CREATE INDEX idx_vehicle_user ON vehicles(user_id);
CREATE INDEX idx_service_vehicle ON service_records(vehicle_id);
CREATE INDEX idx_service_date ON service_records(service_date);
```

### Step 2: Configure Backend

```bash
# Navigate to project directory
cd "d:\23EG107c04\Car Maintainance System\project"

# Open file: src/main/resources/application.properties
# Update the following properties:

spring.datasource.url=jdbc:mysql://localhost:3306/car_maintenance_db
spring.datasource.username=root
spring.datasource.password=YOUR_MYSQL_PASSWORD

# Generate a strong secret key for JWT (use any random string, minimum 256 bits):
jwt.secret=your-super-secret-key-generate-a-strong-value-at-least-256-bits-long-1a2b3c4d5e6f7g8h9i0j1k2l3m4n5o6p
jwt.expiration=86400000

# Other configurations:
spring.jpa.hibernate.ddl-auto=update
server.port=8080
```

**Generate JWT Secret (Optional - Use Online Tool):**
- Go to: https://www.random.org/strings/
- Generate 256-bit string or use any cryptographically secure random string

### Step 3: Build Backend

```bash
# Navigate to project directory
cd "d:\23EG107c04\Car Maintainance System\project"

# Run Maven build
mvn clean install

# This will:
# - Download all dependencies
# - Compile the code
# - Run tests
# - Create JAR file
```

### Step 4: Run Backend

```bash
# Option 1: Using Maven
mvn spring-boot:run

# Option 2: Using Java JAR
java -jar target/project-0.0.1-SNAPSHOT.jar

# You should see:
# "Started ProjectApplication in X seconds"
# "Server is running on port 8080"
```

### Verify Backend

Open browser and test:
```
http://localhost:8080/api/auth/login
```

Should return error (expected, since no auth header):
```json
{
  "timestamp": "...",
  "status": 401,
  "error": "Unauthorized"
}
```

---

## Frontend Setup (React)

### Step 1: Install Dependencies

```bash
# Navigate to frontend directory
cd "d:\23EG107c04\Car Maintainance System\frontend"

# Install npm packages
npm install

# This will install:
# - React 19.2.4
# - React Router DOM
# - Axios
# - Tailwind CSS
# - Chart.js
# - All other dependencies
```

### Step 2: Configure Frontend (Optional)

The frontend automatically connects to `http://localhost:8080/api`

To change API URL, edit: `src/services/api.js`
```javascript
const API_BASE_URL = 'http://localhost:8080/api';
```

### Step 3: Start Frontend Development Server

```bash
# Navigate to frontend directory
cd "d:\23EG107c04\Car Maintainance System\frontend"

# Start development server
npm run dev

# You should see:
# "➜  Local:   http://localhost:5173/"
# "➜  Network: http://xxx.xxx.x.xx:5173/"
```

### Verify Frontend

Open browser:
```
http://localhost:5173
```

Should see login page.

---

## Testing the Complete Application

### Test 1: User Registration

1. Open http://localhost:5173
2. Click "Register here"
3. Fill in:
   - Name: `John Doe`
   - Email: `john@example.com`
   - Password: `password123`
   - Confirm Password: `password123`
4. Click "Register"
5. Should see success message and redirect to dashboard

### Test 2: Add Vehicle

1. Click "Add Vehicle" button
2. Fill in:
   - Vehicle Name: `My Honda`
   - Model: `Honda Civic 2020`
   - Fuel Type: `PETROL`
   - Purchase Date: `2020-05-15`
   - Current KM: `12000`
3. Click "Add Vehicle"
4. Should see vehicle in dashboard with LOW risk

### Test 3: Add Service Record

1. Click "View" on the vehicle
2. Click "+ Add Service"
3. Fill in:
   - Service Date: `2024-03-01`
   - KM at Service: `10000`
   - Service Type: `Oil Change`
   - Notes: `Regular maintenance`
4. Click "Add Service Record"
5. Should see service in history table

### Test 4: Check Prediction

1. Click "Prediction" button
2. Should see:
   - Risk Level: LOW (if recently serviced)
   - Days since service
   - KM since service
   - Maintenance recommendations

### Test 5: Login/Logout

1. Click "Logout" button
2. Should redirect to login page
3. Login with:
   - Email: `john@example.com`
   - Password: `password123`
4. Should see dashboard with your vehicle

---

## Troubleshooting

### Backend Won't Start

**Error: Cannot connect to database**
```
Error: java.sql.SQLException: Cannot get a connection
```
**Solution:**
1. Verify MySQL is running
2. Check username/password in application.properties
3. Ensure database `car_maintenance_db` exists
4. Try connecting manually: `mysql -u root -p -h localhost car_maintenance_db`

**Error: Port 8080 already in use**
```
Address already in use: bind
```
**Solution:**
- Option 1: Stop other application using port 8080
- Option 2: Change port in application.properties: `server.port=8081`
- Option 3: Find process: `netstat -ano | findstr :8080` (Windows)

**Error: Maven build fails**
```
[ERROR] COMPILATION ERROR
```
**Solution:**
1. Ensure Java 17 is installed: `java -version`
2. Clear Maven cache: `mvn clean`
3. Try again: `mvn clean install`

### Frontend Won't Start

**Error: npm install fails**
```
npm ERR! code ERESOLVE
```
**Solution:**
```bash
npm cache clean --force
npm install --legacy-peer-deps
```

**Error: Port 5173 already in use**
```bash
# Start on different port
npm run dev -- --port 3000
```

**Error: Cannot connect to backend**
- Verify backend is running: `http://localhost:8080`
- Check API_BASE_URL in `src/services/api.js`
- Check browser console for CORS errors

### Login Issues

**Error: Invalid email or password**
- Ensure user was registered
- Check email case sensitivity
- Try registering a new user

**Error: Token expired**
- Clear localStorage: Press F12 → Console → `localStorage.clear()`
- Login again

### API Issues

**Error: 401 Unauthorized**
- Token missing or expired
- Clear localStorage and login again
- Check Authorization header format: `Bearer {token}`

**Error: 404 Not Found**
- Endpoint doesn't exist
- Check API endpoint spelling
- Verify backend is running

---

## Running Both Applications Together

### Terminal 1: Backend
```bash
cd "d:\23EG107c04\Car Maintainance System\project"
mvn spring-boot:run
```

### Terminal 2: Frontend
```bash
cd "d:\23EG107c04\Car Maintainance System\frontend"
npm run dev
```

### Open Browser
```
http://localhost:5173
```

---

## Database Queries for Testing

### Check Created Users
```sql
USE car_maintenance_db;
SELECT * FROM users;
```

### Check Vehicles
```sql
SELECT u.email, v.name, v.model, v.km_driven FROM vehicles v
JOIN users u ON v.user_id = u.id;
```

### Check Service Records
```sql
SELECT v.name, sr.service_date, sr.km_at_service, sr.service_type 
FROM service_records sr
JOIN vehicles v ON sr.vehicle_id = v.id;
```

---

## Production Deployment

### Backend Deployment

```bash
# Build JAR
mvn clean package

# Deploy JAR to server
# Set environment variables:
# - DATABASE_URL
# - DATABASE_USER
# - DATABASE_PASSWORD
# - JWT_SECRET

# Run with:
java -jar project-0.0.1-SNAPSHOT.jar
```

### Frontend Deployment

```bash
# Build for production
npm run build

# Output in 'dist' folder
# Deploy to:
# - Vercel
# - Netlify
# - AWS S3 + CloudFront
# - Any static hosting
```

---

## System Architecture

```
┌─────────────────────────┐
│   Browser / Client      │
│   (http://localhost:5173)
└────────────┬────────────┘
             │ HTTP
             │ JSON
             ▼
┌─────────────────────────┐
│   React Frontend        │
│   - Vite Dev Server     │
│   - React Components    │
│   - Axios HTTP Client   │
└────────────┬────────────┘
             │ REST API
             │ CORS Enabled
             ▼
┌─────────────────────────────────────┐
│   Spring Boot Backend               │
│   (http://localhost:8080)           │
│   - REST Controllers    │ Services       │
│   - JPA Repositories    │ DB Access      │
│   - JWT Authentication  │ Security       │
└────────────┬────────────────────────┘
             │ SQL
             │ JDBC
             ▼
┌─────────────────────────┐
│   MySQL Database        │
│   Port: 3306            │
│   - Users               │
│   - Vehicles            │
│   - Service Records     │
└─────────────────────────┘
```

---

## Verification Checklist

- [ ] Java 17 installed and `java -version` works
- [ ] Maven installed and `mvn -version` works
- [ ] MySQL running and accessible
- [ ] `car_maintenance_db` database created
- [ ] Backend `application.properties` configured
- [ ] Backend builds successfully: `mvn clean install`
- [ ] Backend starts: `mvn spring-boot:run`
- [ ] Backend responds: `http://localhost:8080` 
- [ ] Node.js installed and `node -v` works
- [ ] npm installed and `npm -v` works
- [ ] Frontend dependencies installed: `npm install`
- [ ] Frontend starts: `npm run dev`
- [ ] Frontend loads: `http://localhost:5173`
- [ ] Can register new user
- [ ] Can add vehicle
- [ ] Can add service record
- [ ] Can view prediction
- [ ] Can login/logout

---

## Getting Help

### Common Issues
1. Check the main README.md
2. Check BACKEND_README.md in project folder
3. Check FRONTEND_README.md in frontend folder
4. Review browser console (F12)
5. Check backend logs in terminal

### Additional Resources
- Spring Boot Docs: https://spring.io/projects/spring-boot
- React Docs: https://react.dev
- Vite Docs: https://vitejs.dev
- Tailwind CSS: https://tailwindcss.com

---

**✅ Complete! Your application is now ready to use.**
