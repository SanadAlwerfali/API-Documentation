# MUNster (LinkedIn Clone) Notifications Module


In this microservice, we have developed a RESTFUL API that other microservices can utilize to handle notifications across the application. Each API handles a different type of request, including sending notifications, viewing notifications, and checking if a user has un-checked messages or notifications.



## MUNster Notifications REST API - Getting Started

The MUNster Messaging Module provides a suite of REST API's for interacting with the module.

### Public Functions

These API's require a body which contains the functions inputs. See [Public Function Message Format](#munster-rest-api---public-function-message-format) for more info.

The notifications APIs are hosted at the following link https://notifications-5tgazausrq-uc.a.run.app/


| Method   | URL                                                                                        | Perameters                                            | Response                                  | Description                                   |
| -------- | ------------------------------------------------------------------------------------------ | --------------------------------         | ---------------------------------------   | --------------------------------              |
| `GET`   | `/sendNotification`                                                  |user_id<br> notification_content<br> notification_type<br> redirect_url                       | [Send Notification](#send-notification)  | Creates a notification entry for a user       |
| `GET`    | `/viewNotifications`                                | user_id                        | [View Notification](#view-notifications) | Marks the latest notifications as viewed |
| `GET`    | `/getNotifications`                                 | user_id page_number                         | [Get Notification](#get-notifications)   | Retrieves the list of notifications of the user       |
| `GET`    | `/hasNotifications`                                 | user_id                        | [Has Notification](#has-notifications)   | Checks if the user has notifications          |
| `GET`   | `/notifyMessage`                                    | user_id                        | [Notify Message](#notify-message)         | Sets the message notification flag to true for a user     |
| `GET`    | `/hasMessages`                                      | user_id                        | [Has Messages](#has-messages)             | Checks if the user has the message notifications flag set               |
| `GET`    | `/viewMessage`                                | <user_id>                        | [View Message](#view-message) | Resets the message notification flag for a user |

<br> <br> <br>
## MUNster REST API - Public Function Message Format

**Note**: when sending json function inputs through HTTP the following header is required:
```
{
    "Content-Type": "application/json"
}
```

### Send Notification

The request body contains the user_id, notification_type, redirect_url, content. Please note that for the notification_type, use the following as parameters depending on what type of notification it is:

|Type                |parameters  |
| ------------------ | ---------- |
| Job Alert          |    0       |
| Interview Request  |    1       |
| Connection Request |    2       |
| New Connection     |    3       |
| Post Share         |    4       |
| Post Comment       |    5       |
| Post Like          |    6       |

The response is from the server notifying the client if the request was successfully handled or not.

```
Request body:

{
    "user_id": string,
    "notification_type": int,
    "notification_content": string,
    "redirect_url": string
}

Response:

{
    status: 200,
    message: "notification sent"
}
or
{
    status: 400,
    message: "bad request"
}
or
{
    status: 500, 
    message: "internal server error"
}
```

### View Notifications

The request body contains the user_id.

The successful response is the number of notifications marked as viewed, even if 0.

```
Request body:

{
    "user_id": string,
}

Response:

{
    status: 200, 
    message: "marked X notifications as viewed"
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```
### Get Notifications
The request body contains the user_id and the page_number that will be used for pagination purposes. Please note that the page number should be zero for the initial loading of the notification menu.

The response is a list of 5 notification objects per page for the specified user

```
Request body:

{
    "user_id": string,
    "page_number": int,
}

Response:

{
    status: 200,

    message: [{"timestamp":"1667599644", 
            "notification_type":"0", 
            "content":"New job alert for job xyz", 
            "redirect_url":"https://munster.com/jobs/13038"},

            {"timestamp":"1667599766", 
            "notification_type":"2", 
            "content":"New connection request from john doe", 
            "redirect_url":"https://munster.com/connections"},
            
            ...
            
            ]
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```
### Has Notifications

The request body contains the user_id.

The response message is true if the user has un-checked notifications and false otherwise.

```
Request:

{
    "user_id": string
}

Response:

{
    status: 200, 
    message: true | false
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```
### Notify Message
The request body contains the user_id.

The successful response is that the message notification flag has been set to true for the specified user.
```
{
    "user_id": string,
}

Response:

{
    status: 200, 
    message: "message notification flag set"
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```
### Has Messages

The request body contains the user_id.

The response is true if the user has un-checked messages and false otherwise.

```
Request:

{
    "user_id": string,
}

Response:

{
    status: 200, 
    message: true | false
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```

### View Message
The request body contains the user_id.

The successful response is that the message notification flag has been set to false for the specified user.

```
Request body:

{
    "user_id": string,
}

Response:

{
    status: 200, 
    message: message notification flag set
}
or
{
    status: 400,
    message: "Bad request."
}
or
{
    status: 500, 
    message: "internal server error"
}
```

## Development

### NestJs Installation

To install on **Windows**, run:

`npm i -g @nestjs/cli`

You may need to install Node if you don't have it on your machine already.

Install the Firebase CLI using [these instructions](https://firebase.google.com/docs/cli)

Verify that you've installed correctly by running:

`firebase --version`

Log into firebase using:

`firebase login`

To set up the Firebase emulators for testing, run:

`firebase emulators:start`

Hit enter to select default options, then 'y' to download the emulators when prompted.

