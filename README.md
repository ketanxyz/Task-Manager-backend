# Task Manager Backend

## Description

This is the backend API for a full-stack task management application. It provides RESTful APIs for user authentication, task management, user management, and reporting features. Built with Node.js, Express, and MongoDB.

## Repositories

- **Frontend**: https://github.com/ketanxyz/Task-Manager-Frontend.git
- **Backend**: https://github.com/ketanxyz/Task-Manager-backend.git

## Features

- **User Authentication**: JWT-based authentication with role-based access control
- **Task Management**: CRUD operations for tasks with status tracking and checklists
- **User Management**: Admin functionality to manage users
- **Dashboard Data**: Analytics and statistics for admin and user dashboards
- **File Uploads**: Support for profile image uploads
- **Reports**: Export functionality for tasks and user reports in Excel format
- **CORS Support**: Configured for frontend integration

## Tech Stack

- Node.js
- Express.js
- MongoDB with Mongoose
- JWT for authentication
- bcryptjs for password hashing
- Multer for file uploads
- ExcelJS for report generation
- CORS for cross-origin requests

## Prerequisites

- Node.js (version 16 or higher)
- MongoDB database
- npm or yarn

## Installation and Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/ketanxyz/Task-Manager-backend.git
   cd Task-Manager-backend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Set up environment variables**:
   - Create a `.env` file in the root directory with the following variables:
     ```
     MONGO_URL=your_mongodb_connection_string
     JWT_SECRET=your_jwt_secret_key
     CLIENT_URL=http://localhost:5173
     ADMIN_INVITE_TOKEN=4588944
     PORT=8000
     ```

4. **Start the development server**:
   ```bash
   npm run dev
   ```
   The server will run on `http://localhost:8000`

## Available Scripts

- `npm start` - Start the production server
- `npm run dev` - Start the development server with nodemon

## API Documentation

The API follows RESTful conventions and uses JWT for authentication. All protected routes require an `Authorization` header with `Bearer <token>`.

### Authentication Routes (`/api/auth`)

#### Register User
- **Method**: `POST`
- **Endpoint**: `/api/auth/register`
- **Access**: Public
- **Description**: Register a new user account
- **Body**:
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "profileImageUrl": "https://example.com/image.jpg",
    "adminInviteToken": "4588944" // Optional: Use to create admin account
  }
  ```
- **Response**: User data with JWT token

#### Login User
- **Method**: `POST`
- **Endpoint**: `/api/auth/login`
- **Access**: Public
- **Description**: Authenticate user and get JWT token
- **Body**:
  ```json
  {
    "email": "john@example.com",
    "password": "password123"
  }
  ```
- **Response**: User data with JWT token

#### Get User Profile
- **Method**: `GET`
- **Endpoint**: `/api/auth/profile`
- **Access**: Private (JWT required)
- **Description**: Get current user's profile information
- **Headers**: `Authorization: Bearer <token>`
- **Response**: User profile data

#### Update User Profile
- **Method**: `PUT`
- **Endpoint**: `/api/auth/profile`
- **Access**: Private (JWT required)
- **Description**: Update current user's profile
- **Headers**: `Authorization: Bearer <token>`
- **Body**: Updated user fields
- **Response**: Updated user data

#### Upload Image
- **Method**: `POST`
- **Endpoint**: `/api/auth/upload-image`
- **Access**: Public
- **Description**: Upload profile image
- **Content-Type**: `multipart/form-data`
- **Body**: `image` file
- **Response**: `{ "imageUrl": "http://localhost:8000/uploads/filename.jpg" }`

### User Management Routes (`/api/users`)

#### Get All Users
- **Method**: `GET`
- **Endpoint**: `/api/users`
- **Access**: Admin only
- **Description**: Get list of all users
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Array of user objects

####` Get User by ID
- **Method**: `GET
- **Endpoint**: `/api/users/:id`
- **Access**: Private
- **Description**: Get specific user details
- **Headers**: `Authorization: Bearer <token>`
- **Response**: User object

### Task Management Routes (`/api/tasks`)

#### Get Dashboard Data
- **Method**: `GET`
- **Endpoint**: `/api/tasks/dashboard-data`
- **Access**: Private
- **Description**: Get admin dashboard statistics
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Dashboard statistics (total tasks, users, etc.)

#### Get User Dashboard Data
- **Method**: `GET`
- **Endpoint**: `/api/tasks/user-dashboard-data`
- **Access**: Private
- **Description**: Get user-specific dashboard data
- **Headers**: `Authorization: Bearer <token>`
- **Response**: User's task statistics

#### Get Tasks
- **Method**: `GET`
- **Endpoint**: `/api/tasks`
- **Access**: Private
- **Description**: Get tasks (Admin: all tasks, User: assigned tasks)
- **Headers**: `Authorization: Bearer <token>`
- **Query Params**: `status`, `priority`, `assignedTo`, etc.
- **Response**: Array of task objects

#### Get Task by ID
- **Method**: `GET`
- **Endpoint**: `/api/tasks/:id`
- **Access**: Private
- **Description**: Get detailed task information
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Task object with full details

#### Create Task
- **Method**: `POST`
- **Endpoint**: `/api/tasks`
- **Access**: Admin only
- **Description**: Create a new task
- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "title": "Task Title",
    "description": "Task description",
    "assignedTo": "user_id",
    "priority": "high|medium|low",
    "dueDate": "2024-12-31",
    "checklist": ["Item 1", "Item 2"]
  }
  ```
- **Response**: Created task object

#### Update Task
- **Method**: `PUT`
- **Endpoint**: `/api/tasks/:id`
- **Access**: Private
- **Description**: Update task details
- **Headers**: `Authorization: Bearer <token>`
- **Body**: Updated task fields
- **Response**: Updated task object

#### Delete Task
- **Method**: `DELETE`
- **Endpoint**: `/api/tasks/:id`
- **Access**: Admin only
- **Description**: Delete a task
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Success message

#### Update Task Status
- **Method**: `PUT`
- **Endpoint**: `/api/tasks/:id/status`
- **Access**: Private
- **Description**: Update task status
- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "status": "pending|in-progress|completed|cancelled"
  }
  ```
- **Response**: Updated task object

#### Update Task Checklist
- **Method**: `PUT`
- **Endpoint**: `/api/tasks/:id/todo`
- **Access**: Private
- **Description**: Update task checklist items
- **Headers**: `Authorization: Bearer <token>`
- **Body**:
  ```json
  {
    "checklist": [
      {"text": "Item 1", "completed": true},
      {"text": "Item 2", "completed": false}
    ]
  }
  ```
- **Response**: Updated task object

### Report Routes (`/api/reports`)

#### Export Tasks Report
- **Method**: `GET`
- **Endpoint**: `/api/reports/export/tasks`
- **Access**: Admin only
- **Description**: Export all tasks as Excel file
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Excel file download

#### Export Users Report
- **Method**: `GET`
- **Endpoint**: `/api/reports/export/users`
- **Access**: Admin only
- **Description**: Export user-task report as Excel file
- **Headers**: `Authorization: Bearer <token>`
- **Response**: Excel file download

## Integration Steps

### Frontend Integration

1. **Install Axios** in your frontend project:
   ```bash
   npm install axios
   ```

2. **Create API Base Configuration**:
   ```javascript
   // utils/api.js
   import axios from 'axios';

   const API_BASE_URL = 'http://localhost:8000/api';

   const api = axios.create({
     baseURL: API_BASE_URL,
   });

   // Add JWT token to requests
   api.interceptors.request.use((config) => {
     const token = localStorage.getItem('token');
     if (token) {
       config.headers.Authorization = `Bearer ${token}`;
     }
     return config;
   });

   export default api;
   ```

3. **Authentication Example**:
   ```javascript
   // Login
   const login = async (email, password) => {
     const response = await api.post('/auth/login', { email, password });
     localStorage.setItem('token', response.data.token);
     return response.data;
   };

   // Register
   const register = async (userData) => {
     const response = await api.post('/auth/register', userData);
     return response.data;
   };
   ```

4. **Task Management Example**:
   ```javascript
   // Get tasks
   const getTasks = async () => {
     const response = await api.get('/tasks');
     return response.data;
   };

   // Create task (Admin only)
   const createTask = async (taskData) => {
     const response = await api.post('/tasks', taskData);
     return response.data;
   };

   // Update task status
   const updateTaskStatus = async (taskId, status) => {
     const response = await api.put(`/tasks/${taskId}/status`, { status });
     return response.data;
   };
   ```

5. **File Upload Example**:
   ```javascript
   const uploadImage = async (file) => {
     const formData = new FormData();
     formData.append('image', file);
     const response = await api.post('/auth/upload-image', formData, {
       headers: { 'Content-Type': 'multipart/form-data' }
     });
     return response.data.imageUrl;
   };
   ```

6. **Error Handling**:
   ```javascript
   api.interceptors.response.use(
     (response) => response,
     (error) => {
       if (error.response?.status === 401) {
         // Token expired, redirect to login
         localStorage.removeItem('token');
         window.location.href = '/login';
       }
       return Promise.reject(error);
     }
   );
   ```

## Project Structure

```
Backend/
├── config/
│   └── db.js                 # Database connection
├── controllers/
│   ├── authController.js     # Authentication logic
│   ├── userController.js     # User management
│   ├── taskController.js     # Task operations
│   └── reportController.js   # Report generation
├── middlewares/
│   ├── authMiddleware.js     # JWT authentication
│   └── uploadMiddleware.js   # File upload handling
├── models/
│   ├── Task.js               # Task schema
│   └── User.js               # User schema
├── routes/
│   ├── authRoutes.js         # Auth endpoints
│   ├── userRoutes.js         # User endpoints
│   ├── taskRoutes.js         # Task endpoints
│   └── reportRoutes.js       # Report endpoints
├── uploads/                  # Uploaded files
├── .env                      # Environment variables
├── package.json
├── server.js                 # Main server file
└── README.md
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the API endpoints
5. Submit a pull request

## License

This project is licensed under the ISC License.