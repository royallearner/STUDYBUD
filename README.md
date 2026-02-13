# 📚 STUDYBUD - Community Discussion Platform

> **Note:** This project is developed as a learning initiative to enhance my skills in Django web development. It is currently under continuous development with new features being added regularly.

![Project Status](https://img.shields.io/badge/status-under%20development-yellow)
![Django Version](https://img.shields.io/badge/django-6.0-green)
![Python Version](https://img.shields.io/badge/python-3.x-blue)

---

## 📋 Table of Contents
- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Key Features](#key-features)
- [Database Architecture](#database-architecture)
- [Project Structure](#project-structure)
- [Application Flow](#application-flow)
- [Installation Guide](#installation-guide)
- [API Endpoints](#api-endpoints)
- [Screenshots](#screenshots)
- [Configuration](#configuration)
- [Deployment](#deployment)

---

## 🎯 Overview

**STUDYBUD** is a community-driven discussion platform built with Django, where users can create and join topic-based discussion rooms. The platform facilitates knowledge sharing and collaboration by enabling users to:

- Create discussion rooms on various topics
- Join existing rooms and participate in conversations
- Send and manage messages within rooms
- Create and customize user profiles
- Browse topics and recent activities
- Track room participants

This application demonstrates core Django concepts including authentication, database relationships, CRUD operations, REST API development, and deployment configuration.

---

## 🛠️ Tech Stack

### Backend Framework
```
┌─────────────────────────────────────┐
│         Django 6.0                  │
│  Python Web Framework               │
└─────────────────────────────────────┘
```

### Core Technologies

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| **Framework** | Django | 6.0 | Main web framework |
| **Database** | PostgreSQL | - | Production database (via psycopg2-binary 2.9.11) |
| **Storage** | Cloudinary | 1.44.1 | Media file storage |
| **Server** | Gunicorn | 23.0.0 | WSGI HTTP Server |
| **Server** | Uvicorn | 0.40.0 | ASGI server |
| **Static Files** | WhiteNoise | 6.11.0 | Static file serving |
| **Image Processing** | Pillow | 12.0.0 | Image handling |
| **CORS** | django-cors-headers | 4.9.0 | Cross-Origin Resource Sharing |

### Additional Dependencies
- **asgiref** (3.11.0) - ASGI specification implementation
- **dj-database-url** (3.0.1) - Database configuration via URL
- **django-cloudinary-storage** (0.3.0) - Cloudinary integration
- **requests** (2.32.5) - HTTP library

---

## ✨ Key Features

### 1. User Authentication System
```
┌──────────────────────────────────┐
│  ✓ Custom User Registration      │
│  ✓ Email-based Login             │
│  ✓ Logout Functionality          │
│  ✓ Password Validation           │
└──────────────────────────────────┘
```

### 2. Room Management
- **Create Rooms**: Users can create discussion rooms with topics
- **Update Rooms**: Room hosts can edit room details
- **Delete Rooms**: Room hosts can delete their rooms
- **Join Rooms**: Users automatically become participants when messaging
- **Room Filtering**: Search rooms by topic, name, or description

### 3. Messaging System
- Real-time message posting in rooms
- Message deletion by message owner
- Message display with timestamps
- Recent activity tracking

### 4. User Profiles
- Custom user profiles with avatar support
- Bio and personal information
- User activity history
- Profile update functionality

### 5. Topic Browsing
- View all topics
- Filter rooms by topic
- Topic-based search functionality

### 6. Recent Activity Feed
- Global activity stream
- Topic-filtered activities
- User-specific activity tracking

---

## 🗄️ Database Architecture

### Entity Relationship Diagram

```mermaid
erDiagram
    User ||--o{ Room : "hosts"
    User ||--o{ Message : "writes"
    User }o--o{ Room : "participates"
    Topic ||--o{ Room : "categorizes"
    Room ||--o{ Message : "contains"

    User {
        int id PK
        string username
        string name
        string email UK
        string password
        text bio
        image avatar
        datetime date_joined
    }

    Topic {
        int id PK
        string name
    }

    Room {
        int id PK
        int host_id FK
        int topic_id FK
        string name
        text description
        datetime updated
        datetime created
    }

    Message {
        int id PK
        int user_id FK
        int room_id FK
        text body
        datetime updated
        datetime created
    }
```

## 📁 Project Structure

```
STUDYBUD/
│
├── 📁 studybud/                    # Main project directory
│   ├── __init__.py
│   ├── settings.py                # Project settings & configuration
│   ├── urls.py                    # Main URL configuration
│   ├── asgi.py                    # ASGI configuration
│   └── wsgi.py                    # WSGI configuration
│
├── 📁 base/                        # Main application
│   ├── 📁 api/                     # REST API implementation
│   │   ├── __init__.py
│   │   ├── serializers.py         # API serializers
│   │   ├── views.py               # API views
│   │   └── urls.py                # API URL routing
│   │
│   ├── 📁 migrations/              # Database migrations
│   │   ├── __init__.py
│   │   └── 0001_initial.py        # Initial migration
│   │
│   ├── 📁 templates/base/          # HTML templates
│   │   ├── home.html              # Main landing page
│   │   ├── login_register.html    # Authentication page
│   │   ├── room.html              # Room detail page
│   │   ├── room_form.html         # Room creation/editing form
│   │   ├── profile.html           # User profile page
│   │   ├── update-user.html       # Profile update form
│   │   ├── delete.html            # Deletion confirmation
│   │   ├── topics.html            # Topics listing page
│   │   ├── activity.html          # Activity feed page
│   │   ├── feed_component.html    # Feed component
│   │   ├── topics_component.html  # Topics sidebar component
│   │   └── activity_component.html # Activity component
│   │
│   ├── __init__.py
│   ├── models.py                  # Database models
│   ├── views.py                   # View functions
│   ├── urls.py                    # App URL routing
│   ├── forms.py                   # Form classes
│   ├── admin.py                   # Admin configuration
│   ├── apps.py                    # App configuration
│   └── tests.py                   # Test cases
│
├── 📁 templates/                   # Global templates
│   ├── main.html                  # Base template
│   └── navbar.html                # Navigation bar
│
├── 📁 static/                      # Static files
│   ├── 📁 images/                  # Image assets
│   │   ├── logo.svg
│   │   ├── avatar.svg
│   │   ├── favicon.ico
│   │   └── 📁 icons/              # UI icons
│   ├── 📁 styles/                  # CSS files
│   │   ├── style.css              # Main stylesheet
│   │   └── main.css
│   └── 📁 js/                      # JavaScript files
│       └── script.js              # Main script
│
├── 📁 staticfiles/                 # Collected static files (production)
│
├── manage.py                       # Django management script
├── requirements.txt                # Python dependencies
├── build.sh                        # Build script for deployment
└── render.yaml                     # Render deployment configuration
```

---

## 🔄 Application Flow

### 1. User Authentication Flow

```mermaid
flowchart TD
    A[User visits site] --> B{Authenticated?}
    B -->|No| C[Show Login/Register Page]
    B -->|Yes| D[Redirect to Home]
    C --> E{User Action}
    E -->|Login| F[Enter email & password]
    E -->|Register| G[Fill registration form]
    F --> H{Valid credentials?}
    H -->|Yes| I[Create session]
    H -->|No| J[Show error message]
    G --> K{Form valid?}
    K -->|Yes| L[Create user account]
    K -->|No| M[Show validation errors]
    L --> I
    I --> D
    J --> C
    M --> C
```

### 2. Room Creation and Discussion Flow

```mermaid
flowchart TD
    A[Authenticated User] --> B[Click Create Room]
    B --> C[Fill Room Form]
    C --> D{Select/Create Topic}
    D -->|Existing| E[Choose from dropdown]
    D -->|New| F[Enter new topic name]
    E --> G[Enter room name & description]
    F --> G
    G --> H[Submit form]
    H --> I[Room created]
    I --> J[Redirect to home]
    
    K[User browses rooms] --> L{Search/Filter?}
    L -->|Yes| M[Apply filters]
    L -->|No| N[View all rooms]
    M --> O[Filtered room list]
    N --> O
    O --> P[Click room]
    P --> Q[View room details]
    Q --> R[See messages & participants]
    R --> S{Post message?}
    S -->|Yes| T[Type message]
    T --> U[Submit]
    U --> V[Message saved]
    V --> W[Added to participants]
    W --> X[Page refreshes]
    S -->|No| Y[Continue browsing]
```

### 3. Message Management Flow

```mermaid
flowchart TD
    A[User in Room] --> B[View messages]
    B --> C{User's own message?}
    C -->|Yes| D[Delete option visible]
    C -->|No| E[No delete option]
    D --> F{Click delete?}
    F -->|Yes| G[Show confirmation page]
    F -->|No| H[Continue viewing]
    G --> I{Confirm deletion?}
    I -->|Yes| J[Message deleted]
    I -->|No| K[Return to room]
    J --> K
```

### 4. Profile Management Flow

```mermaid
flowchart TD
    A[Authenticated User] --> B[Click profile]
    B --> C[View profile page]
    C --> D[Display user info]
    D --> E[Show user's rooms]
    D --> F[Show user's activities]
    C --> G{Update profile?}
    G -->|Yes| H[Click edit]
    G -->|No| I[Continue browsing]
    H --> J[Profile update form]
    J --> K[Edit fields]
    K --> L{Upload avatar?}
    L -->|Yes| M[Select image file]
    L -->|No| N[Skip avatar]
    M --> O[Submit form]
    N --> O
    O --> P[Profile updated]
    P --> Q[Redirect to profile]
```

### 5. Room Update/Delete Flow

```mermaid
flowchart TD
    A[User views room] --> B{Is room host?}
    B -->|No| C[View only mode]
    B -->|Yes| D[Edit/Delete options visible]
    D --> E{User action}
    E -->|Edit| F[Show room form with data]
    E -->|Delete| G[Show confirmation page]
    F --> H[Modify fields]
    H --> I[Submit changes]
    I --> J[Room updated]
    J --> K[Redirect to home]
    G --> L{Confirm deletion?}
    L -->|Yes| M[Room deleted]
    L -->|No| N[Return to room]
    M --> K
```

---


### Web Application Routes

| Method | Endpoint | View Function | Purpose | Auth Required |
|--------|----------|--------------|---------|---------------|
| GET/POST | `/login/` | `loginPage` | User login | No |
| GET | `/logout/` | `logoutUser` | User logout | No |
| GET/POST | `/register/` | `registerUser` | User registration | No |
| GET | `/` | `home` | Home page with rooms | No |
| GET/POST | `/room/<pk>/` | `room` | Room detail & messaging | No |
| GET | `/profile/<pk>/` | `userProfile` | User profile | No |
| GET/POST | `/create-room/` | `createRoom` | Create new room | Yes |
| GET/POST | `/update-room/<pk>/` | `updateRoom` | Update room | Yes (Host only) |
| GET/POST | `/delete-room/<pk>/` | `deleteRoom` | Delete room | Yes (Host only) |
| GET/POST | `/delete-message/<pk>/` | `deleteMessage` | Delete message | Yes (Owner only) |
| GET/POST | `/update-user/` | `updateUser` | Update user profile | Yes |
| GET | `/topics/` | `topicsPage` | Topics listing | No |
| GET | `/activities/` | `activitiesPage` | Activities feed | No |

---

## 📸 Screenshots

### Home Page
![Home Page](placeholder-home-page.png)
*Main landing page showing available rooms, topics, and recent activities*

### Room Detail
![Room Detail](placeholder-room-detail.png)
*Discussion room showing messages and participants*

### Login/Register
![Authentication](placeholder-login-register.png)
*User authentication interface*

### Create Room
![Create Room](placeholder-create-room.png)
*Room creation form with topic selection*

### User Profile
![User Profile](placeholder-user-profile.png)
*User profile showing rooms and activities*

### Topics Page
![Topics](placeholder-topics-page.png)
*Browse all discussion topics*

### Activity Feed
![Activity Feed](placeholder-activity-feed.png)
*Recent messages across all rooms*

---

## ⚙️ Configuration

### Settings Overview

#### 1. Authentication
```python
# Custom user model
AUTH_USER_MODEL = 'base.User'

# Email-based authentication
USERNAME_FIELD = 'email'
```

#### 2. Database
```python
# Production: PostgreSQL via environment variable
DATABASES = {
    'default': dj_database_url.config(
        default=os.environ.get('DATABASE_URL'),
        conn_max_age=600,
        ssl_require=True
    )
}
```

#### 3. Static Files
```python
# Static files configuration
STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]

# WhiteNoise for static file serving
STATICFILES_STORAGE = 'whitenoise.storage.StaticFilesStorage'
```

#### 4. Media Files
```python
# Cloudinary for media storage
DEFAULT_FILE_STORAGE = 'cloudinary_storage.storage.MediaCloudinaryStorage'
MEDIA_URL = '/images/'
MEDIA_ROOT = BASE_DIR / 'static/images'
```

#### 5. CORS Configuration
```python
# Allow all origins (development)
CORS_ALLOW_ALL_ORIGINS = True
```

#### 6. Installed Apps
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'cloudinary_storage',      # Media file storage
    'cloudinary',              # Cloudinary integration
    'base.apps.BaseConfig',    # Main application
    'rest_framework',          # REST API
    'corsheaders',             # CORS handling
]
```

---

## 🚢 Deployment

### Deployment Platform: Render

#### Configuration File: `render.yaml`
```yaml
databases:
  - name: mysitedb
    plan: free
    databaseName: mysite
    user: mysite

services:
  - type: web
    plan: free
    name: mysite
    runtime: python
    buildCommand: './build.sh'
    startCommand: 'python -m gunicorn mysite.asgi:application -k uvicorn.workers.UvicornWorker'
```

#### Build Script: `build.sh`
```bash
#!/usr/bin/env bash
set -o errexit

# Install dependencies
pip install -r requirements.txt

# Collect static files
python manage.py collectstatic --no-input

# Run database migrations
python manage.py migrate
```

### Deployment Checklist

✅ Environment variables configured:
- SECRET_KEY
- DATABASE_URL
- CLOUDINARY credentials
- RENDER_EXTERNAL_HOSTNAME

✅ Database:
- PostgreSQL database created
- Migrations applied
- Superuser created

✅ Static files:
- Collected using WhiteNoise
- Properly configured in settings

✅ Media files:
- Cloudinary configured
- Upload permissions set

✅ Security:
- DEBUG = False in production
- ALLOWED_HOSTS configured
- CSRF_TRUSTED_ORIGINS set

---

## 📊 Development Concepts Demonstrated

### Django Concepts Applied

1. **MVT Architecture**
   - Models: Database schema definition
   - Views: Business logic and request handling
   - Templates: HTML rendering with Django template language

2. **ORM (Object-Relational Mapping)**
   - Model relationships (ForeignKey, ManyToManyField)
   - QuerySet API usage
   - Database queries with Q objects

3. **Authentication System**
   - Custom user model extending AbstractUser
   - Login/logout functionality
   - User registration
   - Login required decorators

4. **Forms Handling**
   - ModelForm usage
   - Form validation
   - File upload handling (avatars)

5. **URL Routing**
   - URL patterns
   - Named URLs
   - URL parameters

6. **Template Inheritance**
   - Base templates
   - Template blocks
   - Template includes
   - Context processors

7. **Static and Media Files**
   - Static file serving
   - Media file uploads
   - Cloudinary integration
   - WhiteNoise configuration

8. **Admin Interface**
   - Model registration
   - Admin customization

9. **REST API Development**
   - Django REST Framework
   - Serializers
   - API views
   - CORS configuration

10. **Deployment Configuration**
    - Production settings
    - Database configuration
    - Server setup (Gunicorn/Uvicorn)
    - Build scripts

---

## 🎓 Learning Objectives

This project was created to learn and practice:

✓ Django web framework fundamentals
✓ Database design and relationships
✓ User authentication and authorization
✓ CRUD operations implementation
✓ REST API development
✓ Frontend-backend integration
✓ Static and media file management
✓ Deployment and production configuration
✓ Third-party integrations (Cloudinary)
✓ Version control with Git

---

## 🔮 Future Development

*This section will be updated as new features are added*

Potential features under consideration:
- Real-time messaging with WebSockets
- Direct messaging between users
- Room categories and tagging
- Search functionality enhancements
- User notifications system
- File sharing in rooms
- Room moderation features
- User following system
- Mobile responsive improvements

---

## 📝 License

This is an educational project developed for learning purposes.

---

## 🤝 Contributing

As this is a personal learning project, it is not currently open for contributions. However, feedback and suggestions are welcome!

---

## 📧 Contact

*Add your contact information here*

---

**Last Updated:** February 2025
**Version:** 1.0.0 (Under Development)
