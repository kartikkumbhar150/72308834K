# Stage 1

Following are the core sections the platform should have : 

Dashboard feed: Combining all the notifications on the dashboard.
Placement notification : Provide placement notifications.
Events : Provide all the events happening in the clg.
Acedemics : Provide all the acedmics notification , acedmics calender for the students.
Administrative Services : Provide administrative services to the students for various administrative documents, for ex. bonafied certificate etc.
Emergency notifications section : Provide a section where there are emergency , quick updates provided to students.


API Design : 
All the notifications api require an authenticated user, the backend should look for the user identity from the access token.


1. Get Notification Feed : http GET /api/v1/notifications

category : placement, event, result, exam,emergency
status : read,unread
priority : low ,normal,  high, urgent

Response:
{
      "title": "Placement Drive: ABC Technologies",
      "message": "ABC Technologies is visiting campus for final-year CSE students.",
      "category": "placement",
      "priority": "high",
      "source": {
        "department": "Training and Placement Cell",
        "postedBy": "placement_officer"
        }
       "created_At" : "18-05-2026 , 15:00:00"
}    
       

2. Get Notification Details

http : GET /api/v1/notifications/:notificationId


Response:


    "id": "notif_101",
    "title": "Placement Drive: ABC Technologies",
    "message": "ABC Technologies is visiting campus for final-year CSE students.",
    "body": "Detailed instructions, eligibility, venue, documents required, and timeline.",
    "category": "placement",
    "priority": "high",
    "created_At" : "18-05-2026 , 15:00:00"
    "isRead": true,
    "attachments": [
      {
        "name": "Eligibility Criteria.pdf",
        "url": "example.pdf"
      }
    ],
    "source": {
      "department": "Training and Placement Cell",
      "postedBy": "placement_officer"
    }
  }


3. Mark Notification as Read

http : PATCH /api/v1/notifications/:notificationId/read


Response:
{
    "id": "notif_101",
    "isRead": true,
    "readAt": "18-05-2026 , 15:15:00"
}

API Contract

id|string 
title |string 
message | string 
body |string 
category | string 
priority| string 
createdAt | string 
attachments| mutilpart/form data
source |string


Error Response : All apis return a error:

"error": {
    "code": "NOTIFICATION_NOT_FOUND",
    "message": "Notification not found."
  }


# Stage 2

I will use the PostgreSql database for it,
As the data we are using and providing is a structered data , using a relational database will be the best choice for it , as we can use sql also, but postgresql will provide more functionality.


CREATE TABLE departments (
  id BIGINT PRIMARY KEY,
  name VARCHAR(150) NOT NULL UNIQUE,
  created_at TIMESTAMP NDEFAULT NOW()
);

CREATE TABLE users (
  id BIGINT PRIMARY KEY,
  name VARCHAR(150) NOT NULL,
  email VARCHAR(150) NOT NULL UNIQUE,
  role VARCHAR(40), (student, faculty, admin, placement_officer)
  department_id REFERNCES departments(id),
  branch VARCHAR(80),
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE notifications (
  id BIGINT PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  message VARCHAR NOT NULL,
  body VARCHAR,
  category VARCHAR(40),
  priority VARCHAR(20),
  department_id BIGINT REFERENCES departments(id),
  posted_by BIGINT REFERENCES users(id),
  created_at TIMESTAMP DEFAULT NOW()
);

#Stage 3

The sql query provided is accurate , the reason behind the query to be slow is as the data is increased a lot , there will be a lot of api calls to the db. Use indexing we can improve the speed of it but it not not be useful that much .

Solution :
The solution to this is to develop a redis caching layer. Adding a redis database will store the frequent data in the cache layer. When a notification is posted by the admin , the notification will be stored in the redis database. The users will now fetch the notification from the redis cache. This will significantly reduce the database load and will help to get the api calls very fast.


    
