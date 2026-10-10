# 💕 Romance App

A modern, feature-rich dating and matchmaking application built with Python, FastAPI, and React. Connect with people, discover matches, and build meaningful relationships.

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)

## 🌟 Features

- ✅ User authentication & profiles
- ✅ Matchmaking algorithm
- ✅ Swipe/Like/Pass functionality
- ✅ Real-time messaging
- ✅ Profile recommendations
- ✅ Search & filter by interests
- ✅ Admin dashboard
- ✅ Mobile-friendly design

## 🏗️ Project Structure

```text
romance-app/
├── backend/                    # Python FastAPI backend
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py            # FastAPI app entry point
│   │   ├── config.py          # Configuration & env vars
│   │   ├── database.py        # Database connection
│   │   ├── models/            # SQLAlchemy models
│   │   │   ├── __init__.py
│   │   │   ├── user.py
│   │   │   ├── match.py
│   │   │   └── message.py
│   │   ├── schemas/           # Pydantic schemas
│   │   │   ├── __init__.py
│   │   │   ├── user.py
│   │   │   ├── match.py
│   │   │   └── message.py
│   │   ├── routes/            # API endpoints
│   │   │   ├── __init__.py
│   │   │   ├── auth.py        # Login, signup, logout
│   │   │   ├── users.py       # User profiles, search
│   │   │   ├── matches.py     # Matching & swipe logic
│   │   │   ├── messages.py    # Messaging
│   │   │   └── admin.py       # Admin endpoints
│   │   ├── services/          # Business logic
│   │   │   ├── __init__.py
│   │   │   ├── auth_service.py
│   │   │   ├── match_service.py
│   │   │   └── message_service.py
│   │   ├── utils/             # Utilities
│   │   │   ├── __init__.py
│   │   │   ├── security.py    # JWT, hashing
│   │   │   └── validators.py
│   │   └── middleware/        # CORS, logging
│   ├── tests/                 # Unit & integration tests
│   ├── requirements.txt
│   ├── .env.example
│   └── Dockerfile
├── frontend/                  # React frontend
│   ├── src/
│   │   ├── components/
│   │   │   ├── Auth/
│   │   │   ├── Profile/
│   │   │   ├── Matches/
│   │   │   ├── Messages/
│   │   │   └── Admin/
│   │   ├── pages/
│   │   ├── services/
│   │   ├── styles/
│   │   └── App.jsx
│   ├── package.json
│   └── Dockerfile
├── docker-compose.yml
├── .gitignore
├── docs/
│   ├── API.md
│   ├── SETUP.md
│   └── FEATURES.md
└── README.md
```

## 🚀 Quick Start

### Prerequisites
- Python 3.9+
- Node.js 16+
- PostgreSQL 12+
- Docker (optional)

### Backend Setup

```bash
# Clone the repo
git clone https://github.com/strangerletting-tech/romance-app.git
cd romance-app/backend

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your settings

# Run migrations
alembic upgrade head

# Start the server
uvicorn app.main:app --reload
```

Server runs at: `http://localhost:8000`
API docs: `http://localhost:8000/docs`

### Frontend Setup

```bash
cd ../frontend

# Install dependencies
npm install

# Start development server
npm start
```

Frontend runs at: `http://localhost:3000`

### Docker Setup (Recommended)

```bash
cd romance-app

docker-compose up --build
```

Access:
- Frontend: `http://localhost:3000`
- Backend: `http://localhost:8000`
- API Docs: `http://localhost:8000/docs`

## 📚 API Endpoints

### Authentication
- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/logout`
- `POST /api/auth/refresh`

### Users
- `GET /api/users/profile`
- `PUT /api/users/profile`
- `GET /api/users/search`
- `GET /api/users/{id}`
- `POST /api/users/upload-photo`

### Matches
- `GET /api/matches/recommendations`
- `POST /api/matches/like/{user_id}`
- `POST /api/matches/pass/{user_id}`
- `GET /api/matches/mutual`
- `GET /api/matches/history`

### Messages
- `GET /api/messages/conversations`
- `GET /api/messages/{user_id}`
- `POST /api/messages/send`
- `DELETE /api/messages/{message_id}`

### Admin
- `GET /api/admin/stats`
- `GET /api/admin/users`
- `POST /api/admin/users/{id}/ban`
- `DELETE /api/admin/users/{id}`

## 🗄️ Database Schema

### Users Table
```sql
users (
  id UUID PRIMARY KEY,
  email VARCHAR UNIQUE NOT NULL,
  username VARCHAR UNIQUE NOT NULL,
  password_hash VARCHAR NOT NULL,
  first_name VARCHAR,
  last_name VARCHAR,
  bio TEXT,
  age INTEGER,
  gender VARCHAR,
  location VARCHAR,
  interests JSON,
  photos JSONB[],
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  is_active BOOLEAN
)
```

### Matches Table
```sql
matches (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id),
  target_user_id UUID REFERENCES users(id),
  action VARCHAR (like/pass/unmatch),
  created_at TIMESTAMP
)
```

### Messages Table
```sql
messages (
  id UUID PRIMARY KEY,
  from_user_id UUID REFERENCES users(id),
  to_user_id UUID REFERENCES users(id),
  content TEXT NOT NULL,
  is_read BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
)
```

## 🔐 Security Features

- JWT token-based authentication
- Password hashing with bcrypt
- CORS protection
- Rate limiting
- Input validation & sanitization
- SQL injection prevention
- XSS protection

## 🧪 Testing

```bash
cd backend
pytest tests/

pytest --cov=app tests/
```

## 📋 Environment Variables

Create a `.env` file in the backend directory:

```env
DATABASE_URL=postgresql://user:password@localhost/romance_db
SECRET_KEY=your-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
ALLOWED_ORIGINS=http://localhost:3000
DEBUG=True
```

## 🎨 Key Features Explained

### Matchmaking Algorithm
- Recommends users based on shared interests, location, age range, and preferences
- Avoids recommending users already swiped on

### Real-time Messaging
- WebSocket support for live chat
- Read receipts and typing indicators

### Profile System
- Multi-photo uploads
- Bio with interests and preferences
- Verification badges

### Admin Dashboard
- User management
- Ban/suspend accounts
- Platform statistics

## 🚧 Roadmap

- [ ] Video call feature
- [ ] AI-powered bio suggestions
- [ ] Subscription plans
- [ ] Mobile app (React Native)
- [ ] Push notifications
- [ ] Advanced analytics

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License.

## 👨‍💻 Author

**Your Name** - [GitHub](https://github.com/strangerletting-tech) | [LinkedIn](https://linkedin.com)

## 💬 Support

For support, open an issue on GitHub.

---

**Made with ❤️ for meaningful connections**
