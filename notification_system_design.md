# Stage 

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
