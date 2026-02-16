# FindItKIET 🔍

A comprehensive campus lost-and-found system for KIET, featuring a RESTful API backend and native iOS application. Help students and staff reunite with their lost items efficiently and securely.

## 🌟 Features

### Backend API
- 🔐 **Secure Authentication** - JWT-based authentication with refresh token rotation
- 📦 **Item Management** - Create, search, and manage lost/found items
- 📝 **Claim System** - Submit and track claims with proof verification
- 👑 **Admin Dashboard** - Review claims, manage users, and audit logs
- 🖼️ **Image Upload** - Support for item images via Imgur integration
- 🔒 **Security** - Helmet.js, rate limiting, and input validation with Zod
- 📊 **Logging** - Winston-based comprehensive logging system

### iOS App
- 📱 **Native SwiftUI** - Modern, declarative UI with dark mode support
- 🔎 **Smart Search** - Filter by category, status, and keywords
- 📸 **Image Gallery** - View item photos with pinch-to-zoom
- 🎯 **Claim Submission** - Easy claim process with proof documentation
- 👤 **User Profile** - Manage account and view claim history
- ⚡ **Real-time Updates** - Live claim status tracking
- 🎨 **Premium Design** - Glassmorphism effects and smooth animations

## 🛠️ Tech Stack

### Backend
- **Runtime**: Node.js with TypeScript
- **Framework**: Express.js
- **Database**: PostgreSQL with Prisma ORM
- **Authentication**: JWT (jsonwebtoken)
- **File Upload**: Multer + Imgur API
- **Validation**: Zod
- **Security**: Helmet, CORS, express-rate-limit
- **Logging**: Winston

### iOS App
- **Language**: Swift 5.9+
- **UI Framework**: SwiftUI
- **Architecture**: MVVM (Model-View-ViewModel)
- **Networking**: URLSession with async/await
- **Keychain**: For secure token storage
- **Minimum iOS**: 17.0

## 📋 Prerequisites

### Backend
- Node.js 18+ and npm
- PostgreSQL 15+
- Imgur API credentials (optional, for image uploads)

### iOS App
- macOS with Xcode 15+
- iOS Simulator or physical device with iOS 17+

## 🚀 Getting Started

### Backend Setup

1. **Clone the repository**
```bash
git clone https://github.com/pulkit1245/DEV_OPS_PRACTICE_2.git
cd FindItKIET/backend
```

2. **Install dependencies**
```bash
npm install
```

3. **Configure environment variables**

Create a `.env` file in the `backend` directory:
```env
# Database
DATABASE_URL="postgresql://user:password@localhost:5432/finditkiet"

# JWT
JWT_SECRET="your-super-secret-jwt-key-change-this-in-production"
JWT_REFRESH_SECRET="your-super-secret-refresh-key-change-this-in-production"

# Server
PORT=3000
NODE_ENV=development

# Imgur (optional)
IMGUR_CLIENT_ID="your-imgur-client-id"
```

4. **Start PostgreSQL**
```bash
brew services start postgresql@15
```

5. **Create database**
```bash
# Connect to PostgreSQL
psql postgres

# Create database
CREATE DATABASE finditkiet;
\q
```

6. **Run database migrations**
```bash
npx prisma migrate dev
npx prisma generate
```

7. **Start the development server**
```bash
npm run dev
```

The API will be available at `http://localhost:3000`

### iOS App Setup

1. **Navigate to the iOS app directory**
```bash
cd FindItKIET
```

2. **Open in Xcode**
```bash
open FindItKIET.xcodeproj
```

3. **Configure backend URL**

Update the base URL in `APIService.swift` if needed:
```swift
private static let baseURL = "http://localhost:3000/api/v1"
```

4. **Build and Run**
- Select a simulator (iPhone 15 Pro recommended)
- Press `Cmd + R` to build and run

## 📚 API Documentation

### Base URL
```
http://localhost:3000/api/v1
```

### Authentication Endpoints

#### Register
```bash
POST /auth/register
Content-Type: application/json

{
  "email": "user@kiet.edu",
  "password": "password123",
  "name": "John Doe"
}
```

#### Login
```bash
POST /auth/login
Content-Type: application/json

{
  "email": "user@kiet.edu",
  "password": "password123"
}
```

#### Refresh Token
```bash
POST /auth/refresh
Content-Type: application/json

{
  "refreshToken": "your-refresh-token"
}
```

### Item Endpoints

#### List Items
```bash
GET /items?category=ACCESSORIES&status=FOUND&search=wallet
```

#### Get Item Details
```bash
GET /items/:id
```

#### Create Item (Authenticated)
```bash
POST /items
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "title": "Black Wallet",
  "description": "Found in library",
  "category": "ACCESSORIES",
  "status": "FOUND",
  "location": "Library 2nd Floor",
  "reportedAt": "2026-02-03T10:00:00Z",
  "imageUrls": ["https://i.imgur.com/example.jpg"]
}
```

### Claim Endpoints

#### Submit Claim (Authenticated)
```bash
POST /claims
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "itemId": "item-uuid",
  "proof": [
    {
      "type": "DESCRIPTION",
      "value": "Brown leather wallet with student ID"
    }
  ]
}
```

#### Get User Claims (Authenticated)
```bash
GET /claims
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Admin Endpoints

#### Review Claims (Admin only)
```bash
GET /admin/claims
Authorization: Bearer ADMIN_ACCESS_TOKEN
```

#### Update Claim Status (Admin only)
```bash
PATCH /admin/claims/:id
Authorization: Bearer ADMIN_ACCESS_TOKEN
Content-Type: application/json

{
  "status": "APPROVED"
}
```

## 🧪 Testing

See [TESTING.md](./TESTING.md) for comprehensive testing instructions.

### Quick Test
```bash
cd backend
chmod +x test-api.sh
./test-api.sh
```

## 📂 Project Structure

```
FindItKIET/
├── backend/                 # Node.js/Express backend
│   ├── src/
│   │   ├── controllers/    # Request handlers
│   │   ├── middleware/     # Auth, validation, error handling
│   │   ├── routes/         # API routes
│   │   ├── services/       # Business logic
│   │   ├── utils/          # Helper functions
│   │   ├── config/         # Configuration files
│   │   └── server.ts       # Application entry point
│   ├── prisma/
│   │   └── schema.prisma   # Database schema
│   └── package.json
├── FindItKIET/             # iOS SwiftUI app
│   ├── Models/             # Data models
│   ├── Views/              # SwiftUI views
│   ├── ViewModels/         # View models (MVVM)
│   ├── Services/           # API & Keychain services
│   ├── Components/         # Reusable UI components
│   └── Design/             # Design system
├── TESTING.md              # Testing guide
└── README.md               # This file
```

## 🔐 Security Features

- **Password Hashing**: bcrypt with salt rounds
- **JWT Authentication**: Access + refresh token pattern
- **Token Rotation**: Refresh tokens rotate on use
- **Rate Limiting**: Prevent brute force attacks
- **Input Validation**: Zod schema validation
- **Helmet.js**: Security headers
- **CORS**: Configured for specific origins
- **Secure Keychain**: iOS keychain for token storage

## 🎨 Design System

The iOS app features a cohesive design system with:
- Custom color palette with semantic naming
- Reusable component library
- Consistent spacing and typography
- Smooth animations and transitions
- Glassmorphism effects
- Dark mode support

## 👥 User Roles

- **USER**: Can report items and submit claims
- **ADMIN**: Can review claims and manage the system

### Creating an Admin User

```bash
# Connect to PostgreSQL
psql finditkiet

# Update user role
UPDATE "User" SET role = 'ADMIN' WHERE email = 'user@kiet.edu';
```

## 🚢 Deployment

### Backend Deployment

**Environment Variables for Production:**
```env
NODE_ENV=production
DATABASE_URL="your-production-database-url"
JWT_SECRET="strong-random-secret-at-least-32-chars"
JWT_REFRESH_SECRET="another-strong-random-secret"
PORT=3000
```

**Deploy to platforms like:**
- Heroku
- Railway
- Render
- DigitalOcean

### iOS App Deployment

1. Update bundle identifier in Xcode
2. Configure signing & capabilities
3. Archive for distribution
4. Submit to App Store Connect

## 📝 License

MIT License - See LICENSE file for details

## 👨‍💻 Author

Pulkit Verma

## 🙏 Acknowledgments

- KIET Campus for the project inspiration
- Prisma for excellent ORM
- SwiftUI for modern iOS development

## 📞 Support

For issues and questions, please open an issue on GitHub.

---

Made with ❤️ for KIET Campus Community
