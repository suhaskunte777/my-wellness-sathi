# Environment Setup Guide

This guide explains how to configure the environment variables for the separate Quasar frontend and Laravel backend setup.

## Project Structure

```
wellness-athi/
├── server/          # Laravel Backend
│   ├── env.example  # Backend environment template
│   └── .env         # Backend environment (create from template)
└── client/          # Quasar Frontend
    ├── env.example  # Frontend environment template
    └── .env         # Frontend environment (create from template)
```

## Backend Setup (Laravel)

### 1. Create Environment File

```bash
cd server
cp env.example .env
```

### 2. Generate Application Key

```bash
php artisan key:generate
```

### 3. Configure Database

Update the database settings in `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=wellness_sathi
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

### 4. Run Migrations

```bash
php artisan migrate
```

### 5. Install Dependencies

```bash
composer install
```

## Frontend Setup (Quasar)

### 1. Create Environment File

```bash
cd client
cp env.example .env
```

### 2. Install Dependencies

```bash
pnpm install
```

### 3. Configure API Endpoints

The frontend is already configured to use environment variables. Key configurations:

- `VITE_API_BASE_URL`: Backend API base URL
- `VITE_DEV_SERVER_PORT`: Frontend development server port
- `VITE_PROXY_TARGET`: Backend server URL for development proxy

## Key Environment Variables

### Backend (.env)

| Variable                   | Description                       | Default                                       |
| -------------------------- | --------------------------------- | --------------------------------------------- |
| `APP_URL`                  | Backend application URL           | `http://localhost:8000`                       |
| `APP_FRONTEND_URL`         | Frontend application URL          | `http://localhost:9000`                       |
| `SANCTUM_STATEFUL_DOMAINS` | Domains allowed for stateful auth | `localhost:9000,127.0.0.1:9000`               |
| `CORS_ALLOWED_ORIGINS`     | Allowed CORS origins              | `http://localhost:9000,http://127.0.0.1:9000` |

### Frontend (.env)

| Variable               | Description              | Default                     |
| ---------------------- | ------------------------ | --------------------------- |
| `VITE_API_BASE_URL`    | Backend API base URL     | `http://localhost:8000/api` |
| `VITE_DEV_SERVER_PORT` | Frontend dev server port | `9000`                      |
| `VITE_PROXY_TARGET`    | Backend server for proxy | `http://localhost:8000`     |

## Development Workflow

### 1. Start Backend Server

```bash
cd server
php artisan serve
```

The backend will be available at `http://localhost:8000`

### 2. Start Frontend Development Server

```bash
cd client
pnpm dev
```

The frontend will be available at `http://localhost:9000`

### 3. API Communication

The frontend is configured to:
- Proxy API requests to the backend during development
- Use environment variables for API endpoints
- Handle authentication with Sanctum tokens
- Support CORS for cross-origin requests

## Production Configuration

### Backend Production

Update `server/.env` for production:

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://api.yourdomain.com
APP_FRONTEND_URL=https://yourdomain.com
CORS_ALLOWED_ORIGINS=https://yourdomain.com
```

### Frontend Production

Update `client/.env` for production:

```env
VITE_API_BASE_URL=https://api.yourdomain.com/api
VITE_ENABLE_DEBUG_MODE=false
VITE_ENABLE_DEVTOOLS=false
```

## Authentication Flow

1. **Frontend** makes login request to `/api/auth/login`
2. **Backend** validates credentials and returns Sanctum token
3. **Frontend** stores token in localStorage with profile info
4. **Frontend** includes token in `Authorization: Bearer <token>` header
5. **Backend** validates token and returns protected resources

## CORS Configuration

The backend is configured to allow:
- Origins: `http://localhost:9000`, `http://127.0.0.1:9000`
- Methods: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS`
- Headers: `Content-Type`, `Authorization`, `X-Requested-With`, `X-CSRF-TOKEN`
- Credentials: `true` (for cookies and authentication)

## Troubleshooting

### Common Issues

1. **CORS Errors**: Ensure `CORS_ALLOWED_ORIGINS` includes your frontend URL
2. **Authentication Issues**: Check `SANCTUM_STATEFUL_DOMAINS` configuration
3. **API Connection**: Verify `VITE_API_BASE_URL` and proxy settings
4. **Port Conflicts**: Change `VITE_DEV_SERVER_PORT` if port 9000 is in use

### Debug Mode

Enable debug mode in frontend:
```env
VITE_ENABLE_DEBUG_MODE=true
```

This will show detailed API request/response information in the browser console.

## Security Notes

- Never commit `.env` files to version control
- Use strong, unique passwords for database
- Enable HTTPS in production
- Regularly rotate API keys and tokens
- Monitor CORS origins in production

## Additional Configuration

### Database Seeding

```bash
cd server
php artisan db:seed
```

### Queue Workers (if using queues)

```bash
cd server
php artisan queue:work
```

### File Storage

Configure file storage in `server/.env`:
```env
FILESYSTEM_DISK=local
```

For production, consider using cloud storage like AWS S3.


