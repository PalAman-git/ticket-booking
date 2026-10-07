# How to detect if the user is sending the payment request for twice for same ticket?
By implementing idempotency, Client will send the unique idempotency key from the client side along with the request and my server will confirm that whether or not this same payment request is received by checking the database.

# To avoid multiple booking on the same seat I would implement pessimistic locking over optimistic locking.
I am using pessimistic locking because the contention on the same resource is high in my use case.