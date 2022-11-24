# MUNster Messaging Module 


## MUNster Notifications Minimum Viable Product

In this iteration, we have developed a RESTFUL API that other microservices can utilize to handle notifications across the application. Each API handles a different type of request, including sending notifications, viewing notifications, and checking if a user has un-checked messages or notifications. Next iterations after the MVP will include showing the notifications in real-time.

The main logic of the code is available in the `src/app.controller.ts` file, unit tests under `src/app.controller.spec.ts`, and the jenkins file is in the root directory of `MUNster-Notification` repository on Github.


## MUNster Notifications REST API - Getting Started

The MUNster Messaging Module provides a suite of REST API's for interacting with the module.

### Public Functions

These API's require a body which contains the functions inputs. See [Public Function Message Format](#munster-rest-api---public-function-message-format) for more info.


| Method   | URL                                                                                        | Perameters                                            | Response                                  | Description                                   |
| -------- | ------------------------------------------------------------------------------------------ | --------------------------------         | ---------------------------------------   | --------------------------------              |
| `POST`   | `/sendNotification`                                                  |user_id<br> notification_content<br> notification_type<br> redirect_url                       | [Send Notification](#send-notification)  | Creates a notification entry for a user       |
| `GET`    | `/viewNotifications`                                | user_id                        | [View Notification](#view-notifications) | Marks the latest notifications as viewed |
| `GET`    | `/getNotifications`                                 | user_id page_number                         | [Get Notification](#get-notifications)   | Retrieves the list of notifications of the user       |
| `GET`    | `/hasNotifications`                                 | user_id                        | [Has Notification](#has-notifications)   | Checks if the user has notifications          |
| `POST`   | `/notifyMessage`                                    | user_id                        | [Notify Message](#notify-message)         | Sets the message notification flag to true for a user     |
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

The response is a list of notification objects for specified user

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

### Testing

Jenkins has been setup to automatically run all unit tests with each commit to the repository. Logs of successfully running these tests are available at https://sdp.guru:8443/job/MUNster-Organization/job/MUNster-notification/job/PR-7/lastBuild/console. Each function (API endpoint) has been tested by multiple scenarios, 18 total. Additionally, failure of communicating with the database has been tested as a scenario for each api (7 total), bringing up the total to 25 unit tests. The scenarios for each endpoint are the following:

 - `sendNoifications`: users Notifications stored successfully, Notifications can be added to existing records, and that different user Notifications are stored separately
 
 - `viewNotifications`: no errors happen if user has no previous notifications, unviewed notifications are successfully marked as viewed, and that previously viewed notifications do not get processed again. 
 - `getNotifications`: no errors happen if user has no previous notifications, notifications are fetched from the DB in the correct format and order, and that only the specified user's notifications show up in the returned list
 - `hasNotifications`: returns the correct boolean value depending on whether there is or there are no unread notifications
 - `notifyMessage`: users have the flag set to false by default, and users have the flag set to true if there are unread messages
 - `hasMessage`: returns the correct boolean value depending on the unread messages notification flag
 - `viewMessage`: no errors happen if user has no previously set flag, and successful reseting of flag if it exists

## Work In Progress (next submission)
 - Post engagement notifications should go through extra processing. Notifications of post engagement should be combined into one. Users should not receive 100 notifications for 100 likes on a single post, same goes for comments and shares.
 - pagination should be implemented for fetching the database for a user's notifications. only recent 10 should be made available when user calls function for the first time. Further triggers from frontend (on user scroll down) should bring older notifications sequentially.
 - notifications have to be updated in realtime using web sockets. The functions that will be affected are the `getNotifications`, `hasNotifications`, `hasMessage`.
 - It is a best practice to check the parameter types passed in the request. Currently it will fail with a generic error, putting it as the liability of the user to ensure the precondition. Next iterations will provide a more helpful error message as to which parameter is missing/wrong type/etc. 

