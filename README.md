# MUNster Messaging Module 

## MUNster Notifications REST API - Getting Started

The MUNster Messaging Module provides a suite of REST API's for interacting with the module.

### Public Functions

These API's require a body which contains the functions inputs. See [Public Function Message Format](#munster-rest-api---public-function-message-format) for more info.

| Method   | URL                                                                                        | Perameters                                            | Response                                  | Description                                   |
| -------- | ------------------------------------------------------------------------------------------ | --------------------------------         | ---------------------------------------   | --------------------------------              |
| `POST`   | `http://localhost:3000/sendNotification/`                                                  | <user_id>,<notification_type>,<redirect_url>,<content>                       | [Send Notification](#send-notifications)  | Creates a notification entry for a user       |
| `GET`    | `http://127.0.0.1:5000/viewNotifications?user_id={user_id}`                                | <user_id>                        | [View Notification]](#view-notifications) | Checks if the user has viewed the notification|
| `GET`    | `http://localhost:3000/getNotifications?user_id={user_id}`                                 | <user_id>                        | [Get Notification]](#get-notifications)   | Retrieves the notifications of the user       |
| `GET`    | `http://localhost:3000/hasNotifications?user_id={user_id}`                                 | <user_id>                        | [Has Notification]](#has-notifications)   | Checks if the user has notifications          |
| `POST`   | `http://localhost:3000/notifyMessage?user_id={user_id}`                                    | <user_id>                        | [Notify Message](#notify-message)         | Creates a message notification for a user     |
| `GET`    | `http://localhost:3000/hasMessages?user_id={user_id}`                                      | <user_id>                        | [Has Messages](#has-messages)             | Checks if the user has messages               |

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
| Job-Alert          |    0       |
| Interview-Request  |    1       |
| Connection-Request |    2       |
| New-Connection     |    3       |
| Post-Share         |    4       |
| Post-Comment       |    5       |
| Post-Like          |    6       |

The response is from the server notifying the client if the request was successfully handled or not.

```
Request body:

{
    "user_id": string,
    "notification_type": int,
    "redirect_url": string,
    "content": string
}

Response:

"status: 200 - notification has been added for {user_id}"
"status: 300 - {user_id} has been redirected to {redirect_url}"
```

### View Notification

The request body contains the user_id.

The response is "true" if the request was successfully handled "false" otherwise.

```
Request body:

{
    "user_id": string,
}

Response:

"status: 200 - true"
```
### Get Notification
The request body contains the user_id.

The response is a list of notification objects for {user_id}

```
Request body:

{
    "user_id": string,
}

Response:

[status: 200 - list of notifications]
```
### Has Notification

The request body contains the user_id.

The response is true if the user has un-checked notifications and false otherwise.

```
Request:

{
    "user_id": string
}

Response:

[status: 200 - true]
```
### Notify Message
The request body contains the user_id.

The response is true if the user has recieved a message and false otherwise.
```
{
    "user_id": string,
}

Response:

[status: 200 - true]
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

[status: 200 - true]
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

`firebase init emulators`

Hit enter to select default options, then 'y' to download the emulators when prompted.

### Testing

