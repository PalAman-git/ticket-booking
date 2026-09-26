# Requirements

## 1. Users
There are two types of users.

### Customer
- Register
- Login
- Browse events
- View event Details
- View available seats
- Select seats
- Reserve seats
- Make a payment
- View bookings
- Cancel eligible bookings

### Admin
An admin can:
- Create an event
- Update an event
- Cancel an event
- Configure the venue/seating
- View bookings

## 2. Venue
A venue represents a physical location where events happen.

Example: 
PVR Amristar

A venue contains seats.

Seats have:

- Seat number 
- Row 
- Section
- Type

Example:

A1 Premium
A2 Premium
A3 Premium

B1 Regular
B2 Regular

## 3. Event
An event represents a particular occurence.

For example:

Movie: Avengers
Venue : PVR Amritsar
Date: 28 September 2026
Time: 7:30 PM

An event has:
- Name
- Description
- Venue
- Start time
- End time
- Status
- Seat pricing
- State

## 4. Browse events
A customer should be able to:

- View upcoming events
- Filter by date 
- Filter by venue
- View event details

## 5. View Seats
For a particular event, a customer should be able to see:

                SCREEN
        A1  A2    A3     A4   A5
        B1  B2    B3     B4   B5
        C1  C2    C3     C4   C5
        D1  D2    D3     D4   D5

Each seat should indicate whether it is: 
Available
Temporarily reserved
Booked

## 6. Seat Selection
A customer can select one or more seats.
There should be a configurable max booking for customers.

## 7. Seat reservation
This is the most important requirement of the entire project.

When a customer selects seats, they should be temporarily reserved.

It should become HELD for limited amount of time.

If the payment does not complete in that amount of time the seat should be available

## 8. Concurrent booking
If multiple customer select same seat at the same time , the system should ensure that at most one customer can successfully obtain the reservation for A10.

## 9. Booking

After successful reservation, the customer can proceed to booking/payment.

A booking should contain:

- Customer
- Event
- Seats
- Total amount
- Booking status
- Creation time
- Payment information

You decide how these entities should be represented.

A customer should be able to view their booking history.

## 10. Payment
I should make a fake payment service that should return one of the following:
- SUCCESS
- FAILED
- TIMEOUT

## 11. Payment timeout

The fake payment service should occasionally simulate a timeout.

Example:

Your system
     │
     │ Payment request
     ▼
Fake Payment Service
     │
     │ ...............
     │
     └── timeout

What state is the booking in?
Is the seat still reserved?
Can the payment be retried?
What happens if the original payment actually succeeded but your system didn't receive the response?

## 12. Duplicate requests

The client might accidentally send the same request twice.

The system should not accidentally create two bookings or charge the customer twice.

How you achieve this is for you to determine.

## 13. Booking cancellation

A customer should be able to cancel a booking when cancellation is allowed.

For example, you might define:

Cancellation allowed until 2 hours before event.

You decide the exact business rule.

When a booking is cancelled:

BOOKED
   ↓
CANCELLED

and the appropriate seats should become available according to your rules.

## 14. Event cancellation

An admin can cancel an event.

If an event is cancelled:

New bookings must not be allowed.
Existing customers should see that their booking is affected.
Seats should no longer be bookable.

You need to decide what happens to existing payments/bookings.

## 16. Notifications

For the MVP, don't build email/SMS integration.

Instead, create an internal notification mechanism.

A customer should receive notifications for things such as:

Booking confirmed
Payment failed
Event cancelled
Booking cancelled

You can simply expose them through an API.

Later you can turn this into a proper asynchronous notification system.

## 17. Admin requirements

Admin can:

Venue management
Create venue
Define seats
Define seat categories
Event management
Create event
Set event schedule
Set pricing
Cancel event
Monitoring

Admin should be able to see:

Number of bookings
Available seats
Reserved seats
Failed payments

Don't build an elaborate admin dashboard initially.

## 18. Non-functional requirements

These are where I want you to start thinking like a backend engineer.

**NFR-01** — Correctness

The system must maintain seat availability correctly under concurrent requests.

**NFR-02** — Consistency

The system must not allow invalid booking states.

**NFR-03** — Reliability

A temporary failure of a worker/process should not permanently lose a valid reservation or booking.

**NFR-04** — Scalability

The API should be capable of running multiple instances.

Don't specify millions of users. You can choose a realistic MVP target yourself.

**NFR-05** — Observability

You should be able to determine:

Who attempted the booking?
Which event?
Which seats?
What happened?
When did it happen?
Why did it fail?
NFR-06 — Security

Customers must not be able to access another customer's bookings.

**NFR-07** — Recovery

The system should recover correctly after a process restart.


## 19. Failure scenarios

Your system should handle these scenarios:

1. Two users book the same seat simultaneously.

2. User reserves a seat but closes the browser.

3. Reservation expires while another user is trying to book it.

4. Payment fails.

5. Payment service times out.

6. Payment succeeds but your application doesn't receive the response.

7. User clicks "Pay" twice.

8. User sends the booking request twice.

9. API process crashes during booking.

10. Worker crashes while processing an expired reservation.

11. Database connection fails temporarily.

12. Event is cancelled while customers have active reservations.

13. Two application instances try to modify the same seat.

14. Customer attempts to book an already-booked seat.

15. Customer attempts to book after the event has started.