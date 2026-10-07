# Ticket Booking System - HLD 

## Seat Booking

### What happens?
1. User search for events.
2. User choose an Event to book tickets for.
3. User chooses the desired seats.
4. User pay for those seats.
5. Seat is confirmed.

### What data is required?
1. Events
2. Seats for that particular Event.

### How do I respresent availability of same physical seat for different shows?
| ShowSeat |
|---|
| show_id |
| seat_id |
| status |
| held_until |

### When does a seat become unavailable?
Seat becomes unavailable when the payment is successfull.

### What if the user doesn't pay? 
Seat is temporarily in HELD state for 5 min, if payment is not successfull in that time the seat becomes available again.

### How do we release it?
I am going for time based resource unlocking.

### What could go wrong?
1. Backend server could crash.
2. User may click payment twice. [x] used idempotency key
3. Maybe after Payment the server crashes and the payment status is not updated. So would payment be done again?
4. What if Payment Fails?


### What happens if the user sends the same payment request twice?
I will implement idempotency key.

### How do we know whether this is a new payment or a retry of the previous payment?
Using idempotency key , for same logical payment I will have same idempotency key for the backend so as to ensure that the same payment happens only once.


### What if multiple users do it simultaneously? 
1. Double Booking could happen. 
2. One seat can be booked by multiple users at the same time. [x] As there could be high contention on the same resource so I am using pessimisting locking.

### What can fail?
1. Client crashes
2. API server crashes
3. Database crashes
4. Payment provider fails
5. Network timeout
6. Response is lost


### What happens at large scale?
1. Pressure on the backend increases.
2. CPU usage could spike and server could crash.



