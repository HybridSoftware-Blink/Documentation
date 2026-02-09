# Blink Frontend

Modern vehicle fleet management application built with Vue 3, TypeScript, and Tailwind CSS. Connects to a Dockerized Laravel backend API. Features interactive Leaflet maps, secure JWT authentication, and dark mode support.

## Table of Contents
- [Features](#features)
- [Technologies](#technologies)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [API Reference](#api-reference)
- [Testing](#testing)
- [Deployment](#deployment)

## Features

### Current Features
- ✅ **Authentication**: Login, register, JWT token management
- ✅ **User Management**: Full CRUD operations with search and validation
- ✅ **Dashboard**: Main overview with navigation and user profile
- ✅ **Dark Mode**: Theme toggle with localStorage persistence
- ✅ **Responsive UI**: Modern interface with Tailwind CSS

### In Development
- 🔄 **Vehicle Management**: Map integration and CRUD operations
- 🔄 **Landing Page**: Public-facing landing page

### Planned Features
- ⏳ **Advanced Dashboard**: Statistics, charts, and analytics
- ⏳ **Roles & Permissions**: Role-based access control
- ⏳ **Routes/Trips**: Route planning and tracking
- ⏳ **Reports**: Data export and analytics
- ⏳ **Notifications**: Real-time updates system

## Technologies

### Frontend Stack
- **Vue 3.5.24** (Composition API) + **TypeScript**
- **Vite 7.3.1** + **Tailwind CSS 3.4.19**
- **Vue Router 4.6.4** + **Axios 1.13.4**
- **Leaflet** (Interactive maps) + **Heroicons**

### Backend Stack
- **Laravel 12** (PHP 8.2+)
- **MySQL** (MariaDB 11.2) + **MongoDB** (IoT telemetry)
- **Laravel Sanctum** (API authentication)
- **Docker** + **Docker Compose**

### IoT & Hardware
- **Raspberry Pi 4** (Vehicle telemetry collection)

### Development Tools
- **PHPUnit** (Testing) + **Faker** (Test data)
- **Laravel Pint** (Code styling)

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** >= 18.0.0 ([Download](https://nodejs.org/))
- **npm** >= 9.0.0 (included with Node.js)
- **Git** for version control
- **Docker** and **Docker Compose** (for backend)
- **Backend API** must be running at `http://localhost:8001`
  - See backend repository for setup instructions

### Optional
- **VS Code** or preferred code editor
- **Vue DevTools** browser extension for debugging

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/HybridSoftware-Blink/frontend-blink-sprint4.git
cd frontend-blink-sprint4/sprint4-blink
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Start Backend (Required)

Make sure the Laravel backend is running before starting the frontend:

```bash
# In the backend repository directory
docker-compose up -d
docker-compose exec app php artisan migrate
docker-compose exec app php artisan db:seed
```

The backend should be accessible at `http://localhost:8001`

### 4. Start Development Server

```bash
npm run dev
```

The application will be available at `http://localhost:5175`

## Configuration

### Environment Variables

Create a `.env.local` file (optional):

```bash
cp .env.example .env.local
```

**Recommended configuration for development:**

```env
VITE_API_URL=/api
```

This works with Vite's proxy configuration (already set up) and avoids CORS issues.

> **Alternative:** If you prefer direct backend calls with CORS configured:
> ```env
> VITE_API_URL=http://localhost:8001/api
> ```

### Vite Proxy Configuration

The Vite proxy is already configured in `vite.config.ts`:

```typescript
server: {
  proxy: {
    '/api': {
      target: 'http://localhost:8001',
      changeOrigin: true,
    }
  }
}
```

## Usage

### Development Mode

```bash
npm run dev
```

Starts the Vite dev server with hot-reload at `http://localhost:5175`

### Production Build

```bash
npm run build
```

Builds the app for production to the `dist/` folder with TypeScript type checking.

### Preview Production Build

```bash
npm run preview
```

Preview the production build locally before deployment.

### Test Credentials

Use these credentials to test the application (seeded by backend):

**Administrator:**
- Email: `admin@blink.com`
- Password: `password123`
- Role: admin

**Regular User:**
- Email: `john@example.com`
- Password: `password123`
- Role: user

## Project Structure

```
src/
├── components/
│   ├── base/          # Reusable components (Button, Input, Card, etc.)
│   ├── auth/          # Authentication components
│   ├── layout/        # Navbar, Sidebar
│   └── users/         # User-specific components
├── views/             # Pages/Views
│   ├── auth/          # Login, Register
│   ├── dashboard/     # Main dashboard
│   ├── users/         # User management
│   ├── vehicles/      # Vehicle management (in progress)
│   └── settings/      # Settings page
├── composables/       # Reusable composition logic
│   ├── useDarkMode.ts # Dark mode state management
│   ├── useToast.ts    # Toast notifications
│   └── useUser.ts     # User state management
├── services/          # API calls
│   ├── api.service.ts # Axios instance and interceptors
│   ├── auth.service.ts # Authentication API calls
│   └── user.service.ts # User CRUD operations
├── router/            # Vue Router configuration
│   └── index.ts       # Route definitions and guards
├── types/             # TypeScript type definitions
│   ├── auth.types.ts  # Authentication types
│   └── user.types.ts  # User types
└── utils/             # Utility functions
    └── userValidation.ts # User form validation
```

## API Reference

### Base URL
- Development: `http://localhost:8001/api/v1`
- Production: Configure in `.env.local`

### Authentication Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/auth/login` | Login user |
| POST | `/auth/register` | Register new user |
| POST | `/auth/logout` | Logout current user |
| GET | `/auth/me` | Get authenticated user |

### User Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/users` | List all users |
| POST | `/users` | Create new user |
| GET | `/users/{id}` | Get user details |
| PUT | `/users/{id}` | Update user |
| DELETE | `/users/{id}` | Delete user |

### Vehicle Endpoints (In Development)

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/vehicles` | List all vehicles |
| POST | `/vehicles` | Create new vehicle |
| GET | `/vehicles/{id}` | Get vehicle details |
| PUT/PATCH | `/vehicles/{id}` | Update vehicle |
| DELETE | `/vehicles/{id}` | Delete vehicle |
| PATCH | `/vehicles/{id}/location` | Update vehicle location |

### Authentication

The token is stored in `localStorage` as `auth_token` and automatically sent in requests:

```
Authorization: Bearer <token>
```

## Testing

> **Note:** Frontend tests are not yet implemented. The project is currently in active development.

### Backend Tests

To run backend tests:

```bash
# In the backend repository
docker-compose exec app php artisan test

# With coverage
docker-compose exec app php artisan test --coverage
```

## Deployment

### Build for Production

```bash
npm run build
```

This creates optimized production files in the `dist/` folder.

### Deployment Steps

1. **Build the application:**
   ```bash
   npm run build
   ```

2. **Configure environment variables** for production in your hosting platform

3. **Deploy the `dist/` folder** to your static hosting service:
   - Vercel
   - Netlify
   - AWS S3 + CloudFront
   - GitHub Pages
   - Any static web server

4. **Ensure backend API is accessible** from your production frontend

### Environment Variables for Production

Set these in your hosting platform:

```env
VITE_API_URL=https://your-backend-api.com/api
```

---

## License

This project is part of the Blink Sprint 4 development by HybridSoftware-Blink.

## Support

For issues and questions, please contact the development team or create an issue in the repository.
