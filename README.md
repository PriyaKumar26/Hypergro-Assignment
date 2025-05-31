# Hypergro-Assignment

// Project: Property Listing Backend (TypeScript + Node.js)

// --------------------
// FILE: package.json
// --------------------
{
  "name": "property-backend",
  "version": "1.0.0",
  "main": "dist/index.js",
  "scripts": {
    "dev": "ts-node-dev --respawn src/index.ts",
    "build": "tsc"
  },
  "dependencies": {
    "bcrypt": "^5.1.0",
    "dotenv": "^16.3.1",
    "express": "^4.18.2",
    "jsonwebtoken": "^9.0.0",
    "mongoose": "^7.5.0",
    "redis": "^4.6.7"
  },
  "devDependencies": {
    "@types/bcrypt": "^5.0.0",
    "@types/express": "^4.17.21",
    "@types/jsonwebtoken": "^9.0.2",
    "@types/node": "^20.4.9",
    "ts-node-dev": "^2.0.0",
    "typescript": "^5.1.6"
  }
}

// --------------------
// FILE: tsconfig.json
// --------------------
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  },
  "include": ["src"]
}

// --------------------
// FILE: .gitignore
// --------------------
node_modules/
dist/
.env

// --------------------
// FILE: .env.example
// --------------------
MONGO_URI=mongodb://localhost:27017/propertydb
REDIS_URL=redis://localhost:6379
JWT_SECRET=your_jwt_secret

// --------------------
// FILE: src/index.ts
// --------------------
import express from 'express';
import dotenv from 'dotenv';
import { connectMongo } from './config/mongo';
import { connectRedis } from './config/redis';

dotenv.config();
const app = express();

app.use(express.json());

app.get('/', (req, res) => res.send('Property Listing API Running'));

const startServer = async () => {
  await connectMongo();
  await connectRedis();

  app.listen(3000, () => {
    console.log('Server running on http://localhost:3000');
  });
};

startServer();

// --------------------
// FILE: src/config/mongo.ts
// --------------------
import mongoose from 'mongoose';

export const connectMongo = async () => {
  try {
    await mongoose.connect(process.env.MONGO_URI!);
    console.log('✅ MongoDB Connected');
  } catch (error) {
    console.error('❌ MongoDB Connection Failed:', error);
    process.exit(1);
  }
};

// --------------------
// FILE: src/config/redis.ts
// --------------------
import { createClient } from 'redis';

export const redisClient = createClient({
  url: process.env.REDIS_URL
});

export const connectRedis = async () => {
  redisClient.on('error', (err) => console.error('Redis Error:', err));
  await redisClient.connect();
  console.log('✅ Redis Connected');
};

// --------------------
// FILE: README.md
// --------------------
# 🏠 Property Listing Backend (SDE Assignment)

A backend system for managing property listings with authentication, CRUD, favorites, and Redis caching.

## 🚀 Tech Stack
- TypeScript
- Node.js
- Express.js
- MongoDB
- Redis
- JWT Authentication

## 🛠 Setup Instructions

### 1. Clone the repository:
```bash
git clone https://github.com/yourusername/property-backend.git
cd property-backend
```

### 2. Install dependencies:
```bash
npm install
```

### 3. Configure environment variables:
```bash
cp .env.example .env
```
Update `.env` with your MongoDB and Redis connection strings.

### 4. Start the development server:
```bash
npm run dev
```

### 5. Visit:
```
http://localhost:3000
```

## 📮 Available API Routes

| Method | Endpoint           | Description                   |
|--------|--------------------|-------------------------------|
| GET    | `/`                | Base health check             |
| POST   | `/auth/register`   | Register a new user           |
| POST   | `/auth/login`      | Login and get JWT token       |
| POST   | `/properties`      | Create a property (auth)      |
| GET    | `/properties`      | Get all properties (filter)   |
| PUT    | `/properties/:id`  | Update property (auth + owner)|
| DELETE | `/properties/:id`  | Delete property (auth + owner)|
| POST   | `/favorites/:id`   | Add to favorites              |
| GET    | `/favorites`       | Get favorite properties       |
| POST   | `/recommend/:email/:id` | Recommend property      |
| GET    | `/recommendations`| View received recommendations |

## 📦 Deployment (Render/Vercel)
Add your deployment link here once deployed.

## 🎥 Video Demo
Add your recorded video explanation link here.

---

Need help deploying or adding features? Let me know!
