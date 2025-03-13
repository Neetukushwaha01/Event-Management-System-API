# Event-Management-System-API
Event Management System API 
# Event Management System API Documentation

## Overview
The Event Management System API is built using Django and Django REST Framework (DRF) with JWT authentication. It allows users to register, create events, manage attendees, and assign roles with proper permission control.

## Tech Stack
- **Backend:** Django, Django REST Framework
- **Authentication:** JWT (djangorestframework-simplejwt)
- **Database:** PostgreSQL / MySQL
- **Permissions:** Role-based access control (Admin, Organizer, Attendee)

## Roles & Permissions
| Role       | Permissions |
|------------|----------------|
| **Admin** | Can manage all users and events |
| **Organizer** | Can create, update, and delete their own events |
| **Attendee** | Can view events and register for them |

## API Endpoints
### Authentication
| Method | Endpoint | Description | Permission |
|--------|---------|-------------|------------|
| `POST` | `/token/` | Generate JWT token | Public |
| `POST` | `/token/refresh/` | Refresh JWT token | Authenticated User |

### User Management
| Method | Endpoint | Description | Permission |
|--------|---------|-------------|------------|
| `POST` | `/users/register/` | Register new user | Public |
| `GET` | `/users/` | List all users | Admin Only |
| `GET` | `/users/<id>/` | Get user details | Admin / Self |
| `PUT` | `/users/<id>/update/` | Update user | Admin / Self |
| `DELETE` | `/users/<id>/delete/` | Delete user | Admin |

### Event Management
| Method | Endpoint | Description | Permission |
|--------|---------|-------------|------------|
| `POST` | `/events/create/` | Create event | Organizer / Admin |
| `GET` | `/events/` | List all events | Public |
| `GET` | `/events/<id>/` | Get event details | Public |
| `PUT` | `/events/<id>/update/` | Update event | Organizer / Admin |
| `DELETE` | `/events/<id>/delete/` | Delete event | Organizer / Admin |

## Permissions (permissions.py)
```python
from rest_framework import permissions

class IsAdmin(permissions.BasePermission):
    def has_permission(self, request, view):
        return request.user.is_authenticated and request.user.role == 'admin'

class IsOrganizer(permissions.BasePermission):
    def has_permission(self, request, view):
        return request.user.is_authenticated and request.user.role == 'organizer'

class IsAttendee(permissions.BasePermission):
    def has_permission(self, request, view):
        return request.user.is_authenticated and request.user.role == 'attendee'
```

## Views (views.py)
### User Views
```python
from rest_framework import generics, permissions
from .models import User
from .serializers import UserSerializer
from .permissions import IsAdmin

class UserListView(generics.ListAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAdmin]

class UserDetailView(generics.RetrieveAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [IsAdmin]
```

### Event Views
```python
from rest_framework import generics
from .models import Event
from .serializers import EventSerializer
from .permissions import IsOrganizer, IsAdmin

class EventCreateView(generics.CreateAPIView):
    queryset = Event.objects.all()
    serializer_class = EventSerializer
    permission_classes = [IsOrganizer | IsAdmin]

class EventListView(generics.ListAPIView):
    queryset = Event.objects.all()
    serializer_class = EventSerializer
    permission_classes = [permissions.AllowAny]

class EventUpdateView(generics.UpdateAPIView):
    queryset = Event.objects.all()
    serializer_class = EventSerializer
    permission_classes = [IsOrganizer | IsAdmin]
```

## Requirements (requirements.txt)
```plaintext
Django
djangorestframework
djangorestframework-simplejwt
django-filter
psycopg2  # If using PostgreSQL
mysqlclient  # If using MySQL
```

## How to Run the Project
### Install Dependencies
```bash
pip install -r requirements.txt
```
### Run Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```
### Create Superuser
```bash
python manage.py createsuperuser
```
### Start Server
```bash
python manage.py runserver
```

## Summary
- **Authentication:** JWT
- **User Roles:** Admin, Organizer, Attendee
- **Permissions Applied:** `IsAdmin`, `IsOrganizer`, `IsAttendee`
- **Secure API:** Role-based access control
- **Deployment Ready:** Fully modular & scalable

liks this 
admin token - eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzQxODU1NDcyLCJpYXQiOjE3NDE4NTUxNzIsImp0aSI6IjY0YmY4ZTUxMWIxNzRjNDI4OTgyYWViZmQyOWJiNmI2IiwidXNlcl9pZCI6MX0.LCr9PtT-SKpCLnS6GHldvxhevee_DupSDteiHMr6dw4
![image](https://github.com/user-attachments/assets/d46b6dbb-51e0-4a30-a51d-4a52d7dc8d48)



token  urls  liks this 
![image](https://github.com/user-attachments/assets/1154e450-54aa-40ed-ad47-353d679ea40d) use always access token




