# AeroTran

**Team Members**
1. [Abraham Mathewos Meja](https://github.com/abraham-tech)
2. [Leul W. Tewolde](https://github.com/leultewolde)
3. [Msgun Abrham Berhe](https://github.com/Msgun95)
4. [Raimundo Pereira](https://github.com/raipereira)

# Project Description

### Problem Statement

The **Flight Booking System** aims to simplify the flight reservation process, enabling users to book flights efficiently, manage their bookings, and access e-tickets conveniently. The system will cater to two types of users: **Regular Users**, who can search for and book flights, and **Administrators**, who are responsible for managing flight schedules and details.

The system ensures secure authentication and authorization for both Regular Users and Administrators, safeguarding user data and access privileges. Administrators will have the tools to efficiently manage flight schedules, including adding, updating, and removing flights and their associated details.

For Regular Users, the platform will provide an interface to search for flights based on departure and destination locations and availability. During the booking process, users will have access to real-time seat availability and can easily select their preferred seats.

Once a booking is complete, the system generates an e-ticket containing all relevant details. This e-ticket includes a QR code for quick access and validation, enhancing the user experience and streamlining check-ins and other related processes.

### Functional Reqirements

### **1. User Authentication and Authorization**

- Users must be able to create an account on the platform by providing valid credentials.
- Users must be able to log in to their accounts using a secure authentication mechanism.
- Users must be able to log out of their accounts securely.
- There will be two types of users in the system:
    - **Regular Users**: Can search for flights and book seats.
    - **Administrators**: Have extended privileges to manage flights and schedules.

### **2. Flight Management**

- Administrators can:
    - Add valid flights to the system, specifying departure and destination cities, flight numbers, and other relevant details.
    - Update flight schedules, including departure and arrival times or other information.
    - Remove flights and schedules that are no longer valid.

### **3. Flight Search and Selection**

- Regular users can search for flights by providing:
    - Departure city.
    - Destination city.
- The system must display a list of matching flights with details such as departure and arrival times, flight numbers, and seat availability.

### **4. Seat Selection**

- Users must be able to view seat availability on a selected flight.
- Users can choose one or more seats during the booking process.

### **5. Ticket Generation**

- Once a flight is booked, the system must generate an **e-ticket** for the user.
- The e-ticket should contain:
    - User details (name, booking reference).
    - Flight details (flight number, departure/destination cities, times).
    - Seat information.
- The system must provide a downloadable **QR code** with the e-ticket for quick access and verification.

### Filtered Functional Requirements

- A user can create an account on the platform.
- A user can log in and log out of the platform.
- An admin can add valid flights.
- A user can search for flights based on departure and destination cities.
- A user can select a flight and choose seats.
- An admin can Add, update, or remove flight schedules.
- The system should generate e-tickets and provide a downloadable QR code for quick access.

### Good to have:

- Baggage selection and management
- Notification System
- Payment System

### Analysis

### **Entities**

1. **User**
    - Attributes:
        - `user_id` (Primary Key)
        - `name`
        - `email` (unique)
        - `password_hash`
        - `role` ["Regular" | "Admin"]
    - Relationships:
        - A user can book multiple tickets.
        - A user has a role (Regular User or Administrator).
2. **Flight**
    - Attributes:
        - `flight_id` (Primary Key)
        - `flight_number` (unique)
        - `departure_city`
        - `destination_city`
        - `departure_time`
        - `arrival_time`
        - `status` ["Active" | "Delayed" | "Cancelled"]
    - Relationships:
        - A flight can have multiple seats.
3. **Seat**
    - Attributes:
        - `seat_id` (Primary Key)
        - `flight_id` (Foreign Key to Flight)
        - `seat_number`
        - `availability` (e.g., "Available", "Booked")
    - Relationships:
        - A seat belongs to a flight.
        - A seat can be booked as part of a ticket.
4. **Ticket**
    - Attributes:
        - `ticket_id` (Primary Key)
        - `user_id` (Foreign Key to User)
        - `flight_id` (Foreign Key to Flight)
        - `booking_reference` (unique)
        - `seat_ids` (List of seat IDs)
        - `qr_code` (Binary data for QR code)
    - Relationships:
        - A ticket is associated with one user and one flight.
        - A ticket includes one or more seats.

---

### **Relationships**

1. **User and Ticket**
    - **One-to-Many**: A user can book multiple tickets, but a ticket belongs to only one user.
    - Example: A regular user books a ticket for a flight.
2. **Flight and Seat**
    - **One-to-Many**: A flight can have multiple seats, but each seat belongs to only one flight.
    - Example: Flight XYZ123 has 150 seats.
3. **Ticket and Seat**
    - **Many-to-Many**: A ticket can include multiple seats, and a seat can be part of different tickets.
    - Resolution: This can be implemented using a **join table** (`ticket_seat` with `ticket_id` and `seat_id`).
4. **User and Role**
    - **One-to-One**: A user has a single role (Regular or Admin).
    - Example: Admins manage flights, while regular users book flights.
5. **Administrator and Flight**
    - **One-to-Many**: An administrator can manage multiple flights, but each flight is associated with one or more admins.
    - Example: Admin adds or updates flight schedules.

---
## Requirements

### Project Context Diagram
![Context Diagram](context-diagram.png)

### Use Case Diagram
![Use Case Diagram](use-case.png)

### User Story Map
![User Story Map](user-story-map.png)

### Acceptance Criteria and Scenarios
1. **Booking Management: Process Flight Booking**

**Acceptance Criteria:**

- A passenger can select a flight from the available flight schedule.
- The system verifies seat availability before proceeding with booking.
- A successful booking generates a ticket with a unique ID, traveler details, and flight information.
- The system notifies the passenger about successful booking and sends the ticket to their email.

**Scenarios:**

1. **Successful Flight Booking**
    - Given a passenger selects a valid flight
    - And there are available seats on the flight
    - When the passenger confirms the booking
    - Then the system should generate a ticket with all required details
    - And send a confirmation email to the passenger.
2. **Failed Booking Due to Unavailability**
    - Given a passenger selects a flight
    - But there are no available seats on the flight
    - When the passenger attempts to book
    - Then the system should notify the passenger of the unavailability.
3. **Failed Booking Due to System Error**
    - Given a passenger selects a flight
    - And there are available seats
    - But the system encounters an issue while processing
    - When the passenger attempts to confirm the booking
    - Then the system should notify the passenger of the error.

---

2. **Manage Flights: Create New Flight Schedules**

**Acceptance Criteria:**

- Administrative staff can create a new flight schedule by providing all required details (flight number, destination, departure time, etc.).
- The system validates the input data for completeness and correctness.
- A new flight schedule becomes visible to passengers after successful creation.

**Scenarios:**

1. **Successful Flight Schedule Creation**
    - Given an admin provides valid flight details
    - When they submit the information
    - Then the system should create the flight schedule
    - And it should be visible in the list of available flights.
2. **Failed Creation Due to Missing Details**
    - Given an admin provides incomplete flight details
    - When they attempt to create the schedule
    - Then the system should show an error indicating missing information.
3. **Duplicate Flight Schedule Creation**
    - Given an admin attempts to create a schedule with an existing flight number
    - When they submit the information
    - Then the system should reject the creation and notify about the duplication.

---

3. **Integration with Payment Gateways: Make Payment**

**Acceptance Criteria:**

- Passengers can make payments securely via integrated payment gateways.
- The system verifies the payment status and updates the booking accordingly.
- The system provides a receipt for successful transactions.

**Scenarios:**

1. **Successful Payment**
    - Given a passenger proceeds to payment for a booking
    - When they enter valid payment details
    - Then the system should process the payment
    - And generate a receipt.
2. **Failed Payment Due to Insufficient Funds**
    - Given a passenger proceeds to payment
    - When their account lacks sufficient funds
    - Then the system should notify the passenger of the failure.
3. **Failed Payment Due to Gateway Error**
    - Given a passenger proceeds to payment
    - But the payment gateway is down
    - When the passenger attempts to complete payment
    - Then the system should notify the passenger of the error and suggest retrying.
