# Lindsay Precast CAD Workflow Tracker

A full-stack application for tracking CAD workflow projects with MongoDB Atlas integration.

## Project Structure

```
lindsay-cad-tracking-v1/
├── index.html           # Frontend React application
├── .env.local          # Environment variables (MongoDB URI)
└── server/             # Backend Node.js server
    ├── package.json    # Backend dependencies
    └── server.js       # Express server with MongoDB connection
```

## Setup Instructions

### 1. Install Backend Dependencies

Navigate to the server directory and install the required packages:

```powershell
cd server
npm install
```

### 2. Environment Variables

The `.env.local` file in the root directory contains:
- `MONGODB_URI`: Your MongoDB Atlas connection string
- `PORT`: Server port (default: 3000)

### 3. Start the Server

From the `server` directory, run:

```powershell
npm start
```

Or for development with auto-restart (Node v18+):

```powershell
npm run dev
```

The server will:
- Connect to MongoDB Atlas
- Start on http://localhost:3000
- Serve the frontend HTML file
- Provide REST API endpoints

### 4. Access the Application

Open your browser and navigate to:
```
http://localhost:3000
```

## API Endpoints

### Projects
- `GET /api/projects` - Get all projects
- `GET /api/projects/:id` - Get single project
- `POST /api/projects` - Create new project
- `PUT /api/projects/:id` - Update project
- `DELETE /api/projects/:id` - Delete project

### Customers
- `GET /api/customers` - Get all customers
- `POST /api/customers` - Create customer
- `PUT /api/customers/:id` - Update customer
- `DELETE /api/customers/:id` - Delete customer

### Lists (Project Managers, Estimators)
- `GET /api/lists/:type` - Get list by type (pm, estimator, customer)
- `PUT /api/lists/:type` - Update list

## MongoDB Collections

The application uses the following collections in the `lindsay-cad-tracker` database:
- `projects` - CAD project data
- `customers` - Customer information
- `lists` - Project managers, estimators, and other lists

## Connecting Frontend to Backend

To integrate the API with your existing `index.html`, you'll need to:

1. Replace `localStorage` calls with `fetch` API calls
2. Example for loading projects:

```javascript
// Instead of:
const savedProjects = localStorage.getItem("cadProjects");

// Use:
const response = await fetch('/api/projects');
const projects = await response.json();
```

3. Example for saving a project:

```javascript
// Instead of:
localStorage.setItem("cadProjects", JSON.stringify(projects));

// Use:
const response = await fetch('/api/projects', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(projectData)
});
const newProject = await response.json();
```

## Next Steps

1. **Install dependencies**: Run `npm install` in the `server` directory
2. **Start server**: Run `npm start` from the `server` directory
3. **Update frontend**: Modify `index.html` to use the API endpoints instead of localStorage
4. **Test connection**: Verify MongoDB connection in the server console

## Database Schema Examples

### Project Document
```javascript
{
  _id: ObjectId,
  jobNumber: String,
  jobTitle: String,
  customer: String,
  projectManager: String,
  estimator: String,
  dateAssigned: String,
  status: String,
  description: String,
  documents: Array,
  statusHistory: Array,
  createdAt: Date,
  updatedAt: Date
}
```

### Customer Document
```javascript
{
  _id: ObjectId,
  companyName: String,
  contactPerson: String,
  email: String,
  phone: String,
  address: String,
  city: String,
  state: String,
  zip: String,
  notes: String,
  createdAt: Date
}
```

## Troubleshooting

- **Connection Error**: Verify MongoDB URI in `.env.local`
- **Port in Use**: Change PORT in `.env.local`
- **CORS Issues**: The server includes CORS middleware for cross-origin requests
