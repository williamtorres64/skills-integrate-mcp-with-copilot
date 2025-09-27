# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities with **Admin Mode** for teacher authentication.

## Features

- View all available extracurricular activities
- **Admin Mode**: Teacher login/logout system
- **Restricted access**: Only logged-in teachers can register/unregister students
- Interactive web interface with teacher authentication

## Admin Mode

This application implements teacher authentication for secure student management:

- **Teacher Login**: Teachers must authenticate with username/password
- **Restricted Registration**: Only logged-in teachers can sign up students for activities  
- **Restricted Unregistration**: Only logged-in teachers can remove students from activities
- **Teacher Credentials**: Stored in `src/teachers.json`

### Default Teacher Accounts

- Username: `teacher1`, Password: `pass123`
- Username: `teacher2`, Password: `pass456`

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         | Auth Required |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- | ------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count | No            |
| POST   | `/login`                                                          | Teacher login with username/password                                | No            |
| POST   | `/logout`                                                         | Teacher logout                                                      | No            |
| POST   | `/activities/{activity_name}/signup`                              | Sign up a student for an activity                                   | Yes (Teacher) |
| DELETE | `/activities/{activity_name}/unregister`                          | Unregister a student from an activity                               | Yes (Teacher) |

### Authentication

Teacher authentication endpoints accept JSON body:

```json
POST /login
{
  "username": "teacher1", 
  "password": "pass123"
}

POST /logout  
{
  "username": "teacher1"
}
```

Registration endpoints require teacher authentication:

```json
POST /activities/{activity_name}/signup
{
  "email": "student@mergington.edu",
  "teacher": "teacher1"  
}

DELETE /activities/{activity_name}/unregister
{
  "email": "student@mergington.edu",
  "teacher": "teacher1"
}
```

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
