# Car Maintenance Prediction System

A comprehensive full-stack web application designed to help vehicle owners track their vehicle maintenance and predict upcoming servicing needs based on intelligent rule-based logic.

## 🚀 Project Overview

The Car Maintenance Prediction System is a modern, production-ready application that enables users to:
- Register and manage multiple vehicles
- Track service history for each vehicle
- Receive intelligent maintenance predictions with risk levels
- Monitor vehicle health and upcoming service requirements
- Make data-driven decisions about vehicle maintenance

## 📋 Project Structure

```
Car Maintainance System/
├── frontend/                    # React.js + Vite Frontend
│   ├── src/
│   │   ├── pages/             # Page components  
│   │   ├── components/        # Reusable components
│   │   ├── services/          # API configuration
│   │   ├── utils/             # Helper functions
│   │   ├── App.jsx            # Main app with routing
│   │   └── index.css          # Tailwind styles
│   ├── package.json           # Frontend dependencies
│   ├── vite.config.js         # Vite configuration
│   ├── tailwind.config.js     # Tailwind configuration
│   └── FRONTEND_README.md     # Frontend documentation
│
└── project/                     # Spring Boot Backend
  ├── src/
  │   ├── main/java/com/example/project/
  │   │   ├── controller/    # REST API endpoints
  │   │   ├── service/       # Business logic
  │   │   ├── repository/    # Data access layer
  │   │   ├── entity/        # JPA entities
  │   │   ├── dto/           # Data transfer objects
  │   │   ├── security/      # JWT & Security
  │   │   ├── config/        # Configuration classes
  │   │   └── ProjectApplication.java
  │   └── resources/
  │       ├── application.properties  # Configuration
  │       └── db-schema.sql           # Database schema
  ├── pom.xml                # Maven configuration
  ├── Dockerfile             # Docker build instructions
  └── BACKEND_README.md      # Backend documentation
```

## 🛠️ Tech Stack

### Backend
- **Framework**: Spring Boot 4.0.4
- **Language**: Java 17
- **Database**: MySQL 8.0+
- **Authentication**: JWT (JSON Web Tokens)
- **Security**: Spring Security + BCrypt
- **ORM**: Spring Data JPA with Hibernate
- **Build Tool**: Maven

### Frontend
- **Framework**: React.js 19.2.4
- **Build Tool**: Vite 8.0.1
- **Routing**: React Router DOM 6.20.0
- **HTTP Client**: Axios 1.6.2
- **Styling**: Tailwind CSS 3.4.0
- **Charts**: Chart.js & React Chart.js 2

## 📦 System Requirements

### Global Requirements
- Java 17 or higher
- MySQL 8.0 or higher
- Node.js 16.0.0 or higher
- npm or yarn
- Git

## 🚀 Quick Start Guide

### 1. Backend Setup

### 🐳 Run Backend with Docker

You can build and run the backend as a Docker container for easy deployment:

#### Step 1: Build Docker Image
```bash
cd "path/to/project"
docker build -t car-maintenance-backend .
```

#### Step 2: Run Docker Container
```bash
docker run -d -p 8080:8080 --name car-maintenance-backend \
  -e SPRING_DATASOURCE_URL=jdbc:mysql://<host>:<port>/car_maintenance_db \
  -e SPRING_DATASOURCE_USERNAME=<your_mysql_user> \
  -e SPRING_DATASOURCE_PASSWORD=<your_mysql_password> \
  -e JWT_SECRET=<your_jwt_secret> \
  car-maintenance-backend
```

**Notes:**
- Replace environment variable values as needed for your setup.
- The backend will be available at [http://localhost:8080](http://localhost:8080).
- Ensure your MySQL instance is accessible from the container.


#### Step 1: Database Setup
```bash
# Open MySQL command line or MySQL Workbench
# Execute the script from project/src/main/resources/db-schema.sql

CREATE DATABASE IF NOT EXISTS car_maintenance_db;
USE car_maintenance_db;
-- Run all SQL commands from db-schema.sql
```

#### Step 2: Configure Backend
```bash
cd "path/to/project"

# Edit src/main/resources/application.properties
# Update:
# - spring.datasource.password= (your MySQL password)
# - jwt.secret=your-secure-secret-key
```

#### Step 3: Build and Run Backend
```bash
# Install dependencies and build
mvn clean install

# Start the application
mvn spring-boot:run

# Backend will be available at http://localhost:8080
```

### 2. Frontend Setup

#### Step 1: Install Dependencies
```bash
cd "path/to/frontend"
npm install
```

#### Step 2: Configure API (Optional)
- If backend is on different URL, update `src/services/api.js`
- Default: `http://localhost:8080/api`

#### Step 3: Start Development Server
```bash
npm run dev

# Frontend will be available at http://localhost:5173
```

## 📱 Features

### User Authentication
- ✅ User registration with email and password
- ✅ Secure login with JWT tokens
- ✅ Password hashing with BCrypt
- ✅ Automatic token injection in API calls

### Vehicle Management
- ✅ Add multiple vehicles
- ✅ Store vehicle details (name, model, fuel type, purchase date, kilometers)
- ✅ Update vehicle information
- ✅ Delete vehicles
- ✅ View all owned vehicles

### Service History
- ✅ Record service history for each vehicle
- ✅ Track service date and kilometers
- ✅ Add notes about services performed
- ✅ View complete service history in table format
- ✅ Delete service records

### Maintenance Prediction
- ✅ Intelligent prediction logic based on:
  - Kilometers driven since last service (5000 km threshold)
  - Time since last service (6 months threshold)
  - Vehicle age (5 years threshold)
- ✅ Risk level assessment (Low/Medium/High)
- ✅ Personalized maintenance recommendations
- ✅ Service requirement status

### Dashboard
- ✅ Overview of all vehicles
- ✅ Statistics (total vehicles, overdue services, upcoming services)
- ✅ Risk level indicators with color coding
- ✅ Quick actions for each vehicle
- ✅ Responsive design

## 🔒 Security Features

- **Authentication**: JWT-based stateless authentication
- **Password Security**: BCrypt hashing
- **CORS**: Configured for frontend communication
- **Authorization**: Protected routes and endpoints
- **Data Validation**: Input validation on all forms
- **SQL Injection Prevention**: JPA parameterized queries

## 📊 Maintenance Logic

### Risk Levels
1. **LOW** - Green badge
   - KM driven since last service < 3500 km
   - Last service < 4.2 months ago
   - Vehicle age < 5 years

2. **MEDIUM** - Yellow badge
   - KM driven: 3500-7500 km
   - Last service: 4.2-9 months ago

3. **HIGH** - Red badge
   - KM driven > 7500 km
   - Last service > 9 months ago
   - Vehicle age > 5 years

### Service Thresholds
- Service required if: KM > 5000 OR Days > 180 (6 months)
- Additional risk if: Vehicle age > 5 years

## 🔌 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user

### Vehicles
- `POST /api/vehicles` - Create vehicle
- `GET /api/vehicles` - Get all vehicles
- `GET /api/vehicles/{id}` - Get vehicle by ID
- `PUT /api/vehicles/{id}` - Update vehicle
- `DELETE /api/vehicles/{id}` - Delete vehicle

### Service Records
- `POST /api/services/{vehicleId}` - Add service record
- `GET /api/services/vehicle/{vehicleId}` - Get all service records
- `GET /api/services/{id}` - Get service record by ID
- `PUT /api/services/{id}` - Update service record
- `DELETE /api/services/{id}` - Delete service record

### Maintenance
- `GET /api/prediction/{vehicleId}` - Get maintenance prediction

### Dashboard
- `GET /api/dashboard/summary` - Get dashboard summary

## 📝 Sample Data

### Sample User
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

### Sample Vehicle
```json
{
  "name": "My Honda",
  "model": "Honda Civic 2020",
  "fuelType": "PETROL",
  "purchaseDate": "2020-05-15",
  "kmDriven": 45000
}
```

### Sample Service Record
```json
{
  "serviceDate": "2024-03-01",
  "kmAtService": 40000,
  "serviceType": "Regular Service",
  "notes": "Oil change and filter replacement"
}
```

## 🧪 Testing the Application

### Manual Testing Steps
1. **Register**: Create a new account at `/register`
2. **Login**: Login with credentials at `/login`
3. **Add Vehicle**: Click "Add Vehicle" and fill form
4. **View Vehicles**: See all vehicles on dashboard
5. **Add Service**: View vehicle details and add service record
6. **Check Prediction**: Click "Prediction" to see maintenance forecast

## 📚 Documentation

For detailed information, refer to:
- [Backend Documentation](./project/BACKEND_README.md)
- [Frontend Documentation](./frontend/FRONTEND_README.md)

## 🔧 Configuration

### Backend Configuration (application.properties)
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/car_maintenance_db
spring.datasource.username=root
spring.datasource.password=your_password
jwt.secret=your-secret-key
jwt.expiration=86400000
```

### Frontend Configuration (api.js)
```javascript
const API_BASE_URL = 'http://localhost:8080/api';
```

## 🐛 Troubleshooting

### Backend Issues

**Cannot Connect to Database**
- Ensure MySQL is running
- Check database credentials
- Verify `car_maintenance_db` database exists

**Port 8080 Already in Use**
```bash
# Change server.port in application.properties
server.port=8081
```

**JWT Token Issues**
- Check jwt.secret is configured
- Ensure token format is "Bearer {token}"
- Verify token hasn't expired (24 hours)

### Frontend Issues

**Cannot Connect to Backend**
- Ensure backend is running at `http://localhost:8080`
- Check API_BASE_URL in `src/services/api.js`
- Verify CORS is enabled in backend

**Port 5173 Already in Use**
```bash
npm run dev -- --port 3000
```

**npm install fails**
```bash
npm cache clean --force
npm install
```

## 📈 Future Enhancements

- [ ] Email reminders for upcoming services
- [ ] Push notifications
- [ ] Service provider directory
- [ ] Maintenance cost tracking
- [ ] Export history to PDF
- [ ] Graph showing maintenance trends
- [ ] Multi-language support
- [ ] Mobile app version
- [ ] Integration with OBD-II devices
- [ ] Community features (tips, recommendations)

## 📄 License

This project is provided as-is for educational and personal use.

## 👥 Support

For issues or questions:
1. Check the documentation files
2. Review the code comments
3. Check browser console for errors
4. Verify all configurations are correct
5. Ensure both backend and frontend are running

## 🎯 Success Criteria

✅ Full-stack application completed
✅ User authentication implemented
✅ Vehicle management CRUD operations
✅ Service history tracking
✅ Intelligent maintenance prediction logic
✅ Responsive UI with Tailwind CSS
✅ RESTful API design
✅ Database design with relationships
✅ JWT security implementation
✅ Form validation
✅ Error handling
✅ Clean, modular code architecture

---

**Happy Coding!** 🚗💨
