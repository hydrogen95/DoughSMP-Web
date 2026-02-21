# DoughSMP Minecraft Server Website

A complete, production-ready website for the DoughSMP Minecraft server with a full-featured shop system, admin panel, and order management.

![DoughSMP](https://img.shields.io/badge/DoughSMP-Minecraft%20Server-blue)
![Node.js](https://img.shields.io/badge/Node.js-20+-green)
![React](https://img.shields.io/badge/React-19+-cyan)
![License](https://img.shields.io/badge/License-MIT-yellow)

## Features

### Frontend
- Modern React + TypeScript + Tailwind CSS stack
- Responsive design for desktop, tablet, and mobile
- Smooth animations and transitions
- Minecraft-inspired blue theme with glow effects
- Server status display with live player count

### Backend
- Node.js + Express API server
- SQLite database (better-sqlite3) for easy setup
- JWT authentication for admin panel
- File upload support for payment proofs
- RESTful API design

### Shop System
- Browse and purchase donor ranks
- Shopping cart functionality
- GCash payment integration
- Payment proof upload
- Order tracking by Order ID

### Admin Panel
- Secure login with JWT
- Dashboard with order statistics
- Order management (approve/reject)
- Rank management (create/edit/delete)
- Payment settings configuration
- QR code upload for GCash

## Tech Stack

| Category | Technology |
|----------|------------|
| Frontend | React 19, TypeScript, Tailwind CSS, Vite |
| Backend | Node.js, Express |
| Database | SQLite (better-sqlite3) |
| UI Components | shadcn/ui, Radix UI |
| Authentication | JWT (jsonwebtoken) |
| File Upload | Multer |

## Quick Start

### Prerequisites
- Node.js 20+ installed
- npm or yarn package manager

### Installation

1. **Clone or extract the project**
```bash
cd doughsmp-website
```

2. **Install dependencies**
```bash
npm install
```

3. **Configure environment variables**
```bash
cp .env.example .env
# Edit .env and change JWT_SECRET to a secure random string
```

4. **Build the frontend**
```bash
npm run build
```

5. **Start the server**
```bash
npm run server
```

6. **Access the website**
- Website: http://localhost:3001
- Admin Panel: http://localhost:3001/admin/login

### Default Admin Credentials
- **Username:** `admin`
- **Password:** `doughsmp2024`

**Important:** Change the default password after first login!

## Development Mode

To run in development mode with hot reload:

```bash
# Terminal 1: Start the backend server
npm run server:dev

# Terminal 2: Start the frontend dev server
npm run dev
```

## Project Structure

```
doughsmp-website/
├── server/                 # Backend API
│   ├── data/              # SQLite database files
│   ├── uploads/           # Uploaded files (proofs, QR codes)
│   ├── middleware/        # Auth and upload middleware
│   ├── routes/            # API route handlers
│   ├── database.js        # Database setup
│   └── server.js          # Main server file
├── src/                   # Frontend React app
│   ├── components/        # Reusable components
│   ├── hooks/             # Custom React hooks
│   ├── pages/             # Page components
│   ├── types/             # TypeScript types
│   └── index.css          # Global styles
├── dist/                  # Built frontend (generated)
├── .env                   # Environment variables
└── package.json
```

## API Endpoints

### Public Endpoints
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/health` | Health check |
| GET | `/api/ranks` | Get all ranks |
| GET | `/api/ranks/:id` | Get single rank |
| GET | `/api/server/status` | Get server status |
| GET | `/api/server/info` | Get server info |
| GET | `/api/staff` | Get staff list |
| GET | `/api/faq` | Get FAQ items |
| GET | `/api/rules` | Get server rules |
| GET | `/api/settings` | Get public settings |
| GET | `/api/settings/payment` | Get payment info |
| GET | `/api/orders/:orderId` | Get order status |
| POST | `/api/orders` | Create new order |

### Admin Endpoints (Require Authentication)
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/login` | Admin login |
| POST | `/api/auth/change-password` | Change password |
| GET | `/api/orders` | Get all orders |
| GET | `/api/orders/stats/summary` | Get order statistics |
| PUT | `/api/orders/:orderId/status` | Update order status |
| POST | `/api/ranks` | Create rank |
| PUT | `/api/ranks/:id` | Update rank |
| DELETE | `/api/ranks/:id` | Delete rank |
| PUT | `/api/settings/payment` | Update payment settings |
| POST | `/api/settings/qr-code` | Upload QR code |
| GET/POST/PUT/DELETE | `/api/staff` | Staff CRUD |
| GET/POST/PUT/DELETE | `/api/faq` | FAQ CRUD |
| GET/POST/PUT/DELETE | `/api/rules` | Rules CRUD |

## Configuration

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | Server port | `3001` |
| `NODE_ENV` | Environment mode | `development` |
| `JWT_SECRET` | Secret key for JWT | (required) |
| `VITE_API_URL` | API URL for frontend | `http://localhost:3001/api` |

### Customizing Ranks

Ranks can be managed through the admin panel at `/admin/ranks`. You can:
- Create new ranks
- Edit existing ranks (name, price, color, perks)
- Delete ranks (soft delete)

### Changing GCash Details

1. Login to admin panel
2. Go to Settings
3. Update GCash number and account name
4. Upload new QR code if needed

## Database Schema

### Tables
- `ranks` - Available donor ranks
- `orders` - Purchase orders
- `settings` - Website configuration
- `admin_users` - Admin accounts
- `staff_members` - Staff list
- `faq_items` - FAQ entries
- `server_rules` - Server rules

## Security Considerations

1. **Change default admin password** immediately after first login
2. **Use a strong JWT_SECRET** in production (generate with `openssl rand -base64 32`)
3. **Enable HTTPS** in production (use a reverse proxy like Nginx)
4. **Regular backups** of the `server/data/` directory
5. **File upload limits** are set to 5MB for proofs, 2MB for QR codes

## Production Deployment

### Using PM2 (Recommended)
```bash
npm install -g pm2
npm run build
pm2 start server/server.js --name doughsmp
```

### Using Docker
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 3001
CMD ["npm", "run", "server"]
```

### Using Nginx Reverse Proxy
```nginx
server {
    listen 80;
    server_name yourdomain.com;
    
    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

## Troubleshooting

### Database Issues
If you encounter database errors, try:
```bash
rm -rf server/data/
npm run server
```
This will recreate the database with default data.

### Port Already in Use
Change the port in `.env`:
```env
PORT=3002
```

### File Upload Not Working
Ensure the `server/uploads/` directory exists and is writable:
```bash
mkdir -p server/uploads/proofs server/uploads/qr
chmod -R 755 server/uploads
```

## License

MIT License - feel free to use and modify for your own Minecraft server!

## Support

For support, join our Discord: https://discord.gg/doughsmp

---

**Made with love for the DoughSMP Community** ❤️
