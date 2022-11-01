# MUNtour Algorithms Module

## MUNtour REST API - Getting Started

The MUNtour Algorithms Module provIDes a suite of REST API's for interacting with the module.

### Accessing Database

These API's require no body and return database models as json. See [Models](#muntour-rest-api---models) for more info.

| Method   | URL                                                           | Description                      | Response                                |
| -------- | ------------------------------------------------------------- | -------------------------------- | --------------------------------------- |
| `GET`    | `http://127.0.0.1:5000/api/airports/<ID>`                     | Retrieve airport by ID           | [Airport](#airport)                     |
| `GET`    | `http://127.0.0.1:5000/api/flights/<ID>`                      | Retrieve flight by ID            | [Flight](#flight)                       |
| `GET`    | `http://127.0.0.1:5000/api/cars/<ID>`                         | Retrieve car by ID               | [Car](#car)                             |
| `GET`    | `http://127.0.0.1:5000/api/car-rentals/<ID>`                  | Retrieve car rental by ID        | [Car Rental](#car-rental)               |
| `GET`    | `http://127.0.0.1:5000/api/car-rental-agencies/<ID>`          | Retrieve car rental agency by ID | [Car Rental Agency](#car-rental-agency) |
| `GET`    | `http://127.0.0.1:5000/api/hotels/<ID>`                       | Retrieve hotel by ID             | [Hotels](#hotel)                        |
| `GET`    | `http://127.0.0.1:5000/api/hotel-bookings/<ID>`               | Retrieve hotel booking by ID     | [Hotel Bookings](#hotel-booking)        |
| `GET`    | `http://127.0.0.1:5000/api/users/<ID>`                        | Retrieve user by ID              | [Users](#user)                          |
| `GET`    | `http://127.0.0.1:5000/api/bookings/<ID>`                     | Retrieve booking by ID           | [Booking](#booking)                     |

### Public Functions

These API's require a body which contains the functions inputs. See [Public Function Message Format](#muntour-rest-api---public-function-message-format) for more info.

| Method   | URL                                                           | Description                                                            | Message Format                                        |
| -------- | ------------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------- |
| `GET`    | `http://127.0.0.1:5000/api/public/airports/in-area`           | Retrieve all airports in an area                                       | [Airports in Area](#airports-in-area)                 |
| `GET`    | `http://127.0.0.1:5000/api/public/car-rental-agencies/in-area`| Retrieve all car rental agencies in an area                            | [Car Rental Agenies](#car-rental-agencies-in-area)    |
| `GET`    | `http://127.0.0.1:5000/api/public/hotels/in-area`             | Retrieve all hotels in an area                                         | [Hotels in Area](#hotels-in-area)                     |
| `GET`    | `http://127.0.0.1:5000/api/public/hotels/near-airport`        | Retrieve the best bookable hotels near an airport                      | [Hotels Near Airport](#hotels-near-airport)           |
| `GET`    | `http://127.0.0.1:5000/api/public/cars/near-airport`          | Retrieve the best rentable cars near an airport                        | [Cars Near Airport](#cars-near-airport)               |
| `GET`    | `http://127.0.0.1:5000/api/public/flights/between-airports`   | Retrieve the best bookable flight sequences between two airports       | [Flights Between Airports](#flights-between-airports) |
| `GET`    | `http://127.0.0.1:5000/api/public/account/bookings`           | Retrieve all bookings associated with a user                           | [Account Bookings](#account-bookings)                 |
| `POST`   | `http://127.0.0.1:5000/api/public/account/book`               | Book a complete trip. This includes a flight sequence, hotels and cars | [Account Book](#account-book)                         |
| `POST`   | `http://127.0.0.1:5000/api/public/account/login`              | Log in user with email                                                 | [Account Login](#account-login)                       |
| `POST`   | `http://127.0.0.1:5000/api/public/account/signup`             | Create new user with email                                             | [Account Signup](#account-signup)                     |

## MUNtour REST API - Models

The JSON snippets below indicate field names and expected data type.

### Airport
Airport model data is made of a float longitude and latitude, a string airport name, as well as a list of departing flight IDs, arriving flight IDs, nearby hotel IDs, and nearby car rental agency IDs.
```
{
    "latitude": float,
    "longitude": float,
    "name": str,
    "departing_flight_IDs": [str],
    "arriving_flight_IDs": [str],
    "nearby_hotels": [str],
    "nearby_car_rental_agencies": [str]
}
```
### Flight
Flight model data consists of a string departure airport ID and arrival airport ID, as well as a floating point cost.
```
{
    "departure_airport_id": str,
    "arrival_airport_id": str,
    "cost": float
}
```
### Car
Car model data is composed of a string representing the ID of the owner, the car's model as a string, and floating point rental price.
```
{
    "owner_id": str,
    "model": str,
    "rental_price": float
}
```
### Car Rental
Car Rental data encapsulates numerous string types, including the car rental agency ID, a car ID, along with start and end ISO date strings.
```
{
    "start_date": ISO date string,
    "end_date": ISO date string,
    "car_id": str,
    "rental_agency_id": str
}
```
### Car Rental Agency
Car Rental agency data is made of a list of cars composed of string car IDs, as well as a floating point longitude and latitude, and a string name for the agency.
```
{
    "latitude": float,
    "longitude": float,
    "name": str,
    "cars": [str]
}
```
### Hotel
Hotel data is a floating point cost per night, longitude and latitude, as well as a string name for the hotel.
```
{
    "latitude": float,
    "longitude": float,
    "name": str,
    "cost_per_night": float
}
```
### Hotel Booking
Hotel Booking data contains a string start and end date in ISO date string format. It also returns a string hotel ID, as well as an int for number of nights booked.
```
{
    "start_date": ISO date string,
    "end_date": ISO date string,
    "hotel_id": str,
    "nights_booked": int
}
```
### User
User data is composed a string email, along with a list of booking IDs which correlate with booking data specified below.
```
{
    "email": str,
    "bookings": List[str]
}
```
### Booking
Booking data is made up by a list of string car rental IDs, flight IDs, hotel IDs, an owner ID, and a floating point total cost. Note that the data correlated with the IDs are detailed above.
```
{
    "owner_id": str,
    "flight_ids": List[str],
    "car_rental_ids": List[str],
    "hotel_booking_ids": List[str],
    "total_price": float
}
```

## MUNtour REST API - Public Function Message Format

**Note**: when sending json function inputs through HTTP the following header is required:
```
{
    "Content-Type": "application/json"
}
```

It is also important to note that all public function interfaces integrated with the server GET and POST requests make use of a conversion function node_list_to_json_dict(node(s)). This conversion function translates database node data to a standard json dictionary in their responses.

### Airports in Area

The request body contains the longitude and latitude of the upper-left-hand corner of a rectangular area, and the width and height of that area.

The response is a list of [Airport](#airport) objects that are located within the specified area.

```
Request body:

{
    "latitude": float,
    "longitude": float,
    "width": float,
    "height": float
}

Response:

[Airport]
```

### Car Rental Agencies in Area

The request body contains the longitude and latitude of the upper-left-hand corner of a rectangular area, and the width and height of that area.

The response is a list of [Car Rental Agency](#car-rental-agency) objects that are located within the specified area.

```
Request body:

{
    "latitude": float,
    "longitude": float,
    "width": float,
    "height": float
}

Response:

[Car Rental Agency]
```
### Hotels in Area
The request body contains the longitude and latitude of the upper-left-hand corner of a rectangular area, and the width and height of that area.

The response is a list of [Hotel](#hotel) objects that are located within the specified area.

```
Request body:

{
    "latitude": float,
    "longitude": float,
    "width": float,
    "height": float
}

Response:

[Hotel]
```
### Hotels Near Airport

The request body contains an airport ID, search radius, maximum number of results, sort method (either "cost" or "distance"), and start/end date of the stay.

The response is a list of length less than or equal to `max_result_length` of [Hotel](#hotel) objects within `search_radius` from the airport. These hotels will be available for a stay between the start and end times, and will be sorted by either cost or distance from the airport, depending on `sort_method`.

```
Request:

{
    "airport_id": str,
    "search_radius": float,
    "max_result_length": int,
    "sort_method": str {"cost", "distance"},
    "start_date": ISO date string,
    "end_date": ISO date string
}

Response:

[Hotel]
```
### Cars Near Airport
The request body contains an airport ID, search radius, maximum number of results, sort method (either "cost" or "distance"), and start/end date of the rental.

The response is a list of length less than or equal to `max_result_length` of [Car](#car) objects within `search_radius` from the airport. These cars will be available for rental between the start and end times, and will be sorted by either cost or distance from the airport, depending on `sort_method`.
```
{
    "airport_id": str,
    "search_radius": float,
    "max_result_length": int,
    "sort_method": str {"cost", "distance"},
    "start_date": ISO date string,
    "end_date": ISO date string
}

Response:

[Car]
```
### Flights Between Airports

The request body contains IDs for start/end airports, a sort method (either "cost" or "distance"), a maximum number of results, and a departure date.

The response contains a list of lists of [Flight](#flight) objects, where each sub-list represents a flight path from the start to the end.

```
Request:

{
    "start_airport": str,
    "end_airport": str,
    "sort_method": str {"cost", "distance"},
    "max_result_length": int,
    "latest_arrival_date": ISO date string
}

Response:

[[Flight]]
```
### Account Bookings

The request body contains the ID of the user whose bookings are being fetched.

The response body contains a list of [Booking](#booking) objects belonging to the specified user.

```
Request:

{
    "user_id": str
}

Response:

[Booking]
```

**Unlike the above functionality used regarding GET requests, the following Account functionalities provide POST requests, pushing the data to the server.**

### Account Book

The request body contains a list of flight, car rental, and hotel booking IDs, plus the ID of the user who is doing the booking.

The response body contains the booking ID.

```
Request:

{
    "user_id": str,
    "flight_ids": [str],
    "car_rental_ids": [str],
    "hotel_booking_ids": [str]
}

Response:

{
    "booking_id": str
}
```

### Account Login

The request body contains the email of the user who wishes to log in.

The response contains the user's ID.

```
Request:

{
    "email": str
}

Response:

{
    "user_id": str
}
```
### Account Signup

The request body contains the email of the user who wishes to sign up.

The response contains the user's new ID.

```
Request:

{
    "email": str
}

Response:

{
    "user_id": str
}
```

## Development

### Installation

To install on **macOS** or **Linux**, run:

`pip install -e .`

To install on **Windows**, run:

`py -m pip install -e .`

You may need to use `pip3.x` rather than `pip`, depending on your installation.

Install the Firebase CLI using [these instructions](https://firebase.google.com/docs/cli)

Verify that you've installed correctly by running:

`firebase --version`

Log into firebase using:

`firebase login`

To set up the Firebase emulators for testing, run:

`firebase init emulators`

Hit enter to select default options, then 'y' to download the emulators when prompted.

### Testing

To run the unit tests on **macOS** or **Linux**, run:

`firebase emulators:exec "pytest"`

To run the unit tests on **Windows**, run:

`firebase emulators:exec "py -m pytest"`

You may need to use `python3.x` rather than `python`, depending on your installation.

### Autoformatting

To format the repo on **macOS** or **Linux**, run:

`yapf -ir .`

To format the repo on **Windows**, run:

`py -m yapf -ir .`
