# MessagingApp

A real-time messaging application built with Spring Boot and MongoDB, featuring user authentication, friend management, and secure messaging capabilities.

**Authors:** Dima Bondar and Keelia Mattison  
**Course:** CST-339

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [API Endpoints](#api-endpoints)
- [Security](#security)
- [Database Schema](#database-schema)
- [Usage Guide](#usage-guide)
- [Contributing](#contributing)

## Overview

MessagingApp is a web-based messaging platform that allows users to connect with friends and exchange messages in real-time. The application implements a friend request system to ensure users can only communicate with approved contacts, providing a safe and controlled messaging environment.

## Features

### User Management
- **User Registration**: Create new accounts with username, email, and password
- **Secure Authentication**: Login with encrypted credentials using BCrypt
- **Profile Management**: Display names and user information

### Friend System
- **Send Friend Requests**: Invite other users to become friends
- **Accept/Reject Requests**: Manage incoming friend requests
- **View Friend Lists**: See all current friends
- **Track Sent Requests**: Monitor outgoing friend requests

### Messaging
- **Private Messaging**: Send messages to friends only
- **Message History**: View complete conversation history with each friend
- **Chronological Ordering**: Messages sorted by timestamp
- **Real-time Updates**: Access latest messages on page refresh

### Security
- **Password Encryption**: BCrypt hashing for secure password storage
- **Authentication Required**: Protected routes requiring login
- **Friend Validation**: Messages only allowed between confirmed friends
- **CSRF Protection**: Security against cross-site request forgery

## Technology Stack

- **Backend Framework**: Spring Boot 3.x
- **Security**: Spring Security with BCrypt
- **Database**: MongoDB
- **Language**: Java 17+
- **Build Tool**: Maven
- **Template Engine**: Thymeleaf (implied from MVC structure)

## Prerequisites

Before running this application, ensure you have the following installed:

- Java Development Kit (JDK) 17 or higher
- Maven 3.6+
- MongoDB 4.4+ (running locally or accessible remotely)
- Git (for cloning the repository)

## Project Structure

```
src/main/java/com/gcu/messagingapp/
├── config/
│   └── SecurityConfig.java          # Spring Security configuration
├── controller/
│   ├── FriendController.java        # Friend management endpoints
│   ├── HomeController.java          # Home and navigation
│   ├── MessageController.java       # Message handling
│   └── UserController.java          # User registration
├── model/
│   ├── FriendRequest.java           # Friend request entity
│   ├── FriendRequestStatus.java     # Request status enum
│   ├── Message.java                 # Message entity
│   └── User.java                    # User entity
├── repository/
│   ├── FriendRequestRepository.java # Friend request data access
│   ├── MessageRepository.java       # Message data access
│   └── UserRepository.java          # User data access
├── service/
│   ├── FriendService.java           # Friend business logic
│   ├── MessageService.java          # Message business logic
│   └── UserService.java             # User business logic
└── MessagingAppApplication.java     # Application entry point
```

## API Endpoints

### Authentication
- `GET /login` - Display login page
- `POST /login` - Process login (handled by Spring Security)
- `GET /logout` - Logout user
- `GET /register` - Display registration form
- `POST /register` - Register new user

### Messages
- `GET /messages` - View messages page
- `GET /messages?friendId={id}` - View conversation with specific friend
- `POST /messages/send` - Send a new message

### Friends
- `GET /friends` - View friends page with lists and requests
- `POST /friends/request` - Send friend request
- `POST /friends/request/{requestId}` - Accept/reject friend request

### Home
- `GET /` - Redirect to messages page

## Security

### Authentication Flow
1. User accesses protected route
2. Redirected to `/login` if not authenticated
3. Credentials validated against MongoDB
4. Session created upon successful login
5. Access granted to protected resources

### Password Security
- Passwords hashed using BCrypt with salt
- Original passwords never stored
- Secure comparison during authentication

### Authorization
- All routes except `/login` and `/register` require authentication
- Friend validation ensures messages sent only to confirmed friends
- User isolation prevents unauthorized data access

## Database Schema

### Users Collection
```javascript
{
  _id: ObjectId,
  username: String,
  password: String,        // BCrypt hashed
  email: String,
  displayName: String,
  friends: [String]        // Array of friend user IDs
}
```

### Messages Collection
```javascript
{
  _id: ObjectId,
  senderId: String,
  receiverId: String,
  content: String,
  timestamp: ISODate
}
```

### FriendRequests Collection
```javascript
{
  _id: ObjectId,
  senderId: String,
  receiverId: String,
  status: String,          // PENDING, ACCEPTED, REJECTED
  createdAt: ISODate
}
```

## Usage Guide

### Getting Started

1. **Register an Account**
   - Navigate to `/register`
   - Provide username, email, and password
   - Submit to create account

2. **Login**
   - Use credentials to login at `/login`
   - Redirected to messages page upon success

3. **Add Friends**
   - Go to Friends page (`/friends`)
   - Browse available users
   - Send friend requests
   - Wait for acceptance

4. **Send Messages**
   - Navigate to Messages page
   - Select a friend from the list
   - Type message and send
   - View conversation history

### Friend Request Workflow

**Sending Requests:**
1. View available users on Friends page
2. Click to send friend request
3. Request appears in "Sent Requests" section

**Receiving Requests:**
1. Incoming requests appear in "Pending Requests"
2. Accept or reject each request
3. Accepted friends added to friends list

## Development Notes

### Key Design Decisions

- **Friend-Only Messaging**: Messages restricted to confirmed friends for privacy
- **Bidirectional Friend Lists**: Both users maintain friend reference for efficiency
- **Timestamp Ordering**: Messages sorted chronologically for natural conversation flow
- **Status Tracking**: Friend request states prevent duplicate requests

## License

This project is created for educational purposes as part of CST-339 coursework.

---

**Note**: This application is designed for educational purposes and may require additional security hardening and features for production deployment.
