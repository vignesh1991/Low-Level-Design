# Database Design for Class Diagram

## Tables:

1. **Booking**
   - `id` (Primary Key)
   - `show_id` (Foreign Key referencing Show)
   - `user`
   - `booking_status`
   - `confirmed` (Boolean)
   - Other attributes (timestamps, etc.)

2. **Show**
   - `id` (Primary Key)
   - `movie_id` (Foreign Key referencing Movie)
   - `screen_id` (Foreign Key referencing Screen)
   - `start_time`
   - `duration`
   - Other attributes

3. **Movie**
   - `id` (Primary Key)
   - `name`
   - Other attributes

4. **Theatre**
   - `id` (Primary Key)
   - `name`
   - Other attributes

5. **Screen**
   - `id` (Primary Key)
   - `theatre_id` (Foreign Key referencing Theatre)
   - `name`
   - Other attributes

6. **Seat**
   - `id` (Primary Key)
   - `screen_id` (Foreign Key referencing Screen)
   - `row_no`
   - `seat_no`
   - Other attributes

## Relationships:
- `Booking` has a Many-to-One relationship with `Show` (Multiple bookings for one show)
- `Show` has Many-to-One relationships with `Movie` and `Screen`
- `Screen` has a Many-to-One relationship with `Theatre`
- `Seat` has a Many-to-One relationship with `Screen`

