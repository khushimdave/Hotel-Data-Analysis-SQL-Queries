```sql
SELECT * FROM HOTEL
```
![HOTEL table](C:\Users\Khushi\OneDrive\Pictures\Screenshots\Screenshot (661).png)

```sql
SELECT * FROM ROOM
```
```sql
SELECT * FROM GUEST
```
```sql
SELECT * FROM BOOKING
```
## List full details of all hotels.
```sql
SELECT * FROM HOTEL
```

## List full details of all hotels in London.
```sql
SELECT * FROM HOTEL WHERE ADDRESS='London'
```
## List the names and addresses of all guests in London, alphabetically ordered by name.
```sql
SELECT NAME,ADDRESS FROM GUEST WHERE guest_no IN (
SELECT guest_no FROM BOOKING WHERE hotel_no IN (
SELECT hotel_no FROM HOTEL WHERE ADDRESS='London'))ORDER BY name;
```

## List all double or family rooms with a price below £40.00 per night, in ascending order of price.
```sql
SELECT * FROM room WHERE (type = 'Suite' OR type = 'Deluxe') AND price < 40.00 ORDER BY price ASC;
```

## List the bookings for which no date_to has been specified.
```sql
SELECT * FROM BOOKING WHERE date_to IS NULL
```

## How many hotels are there?
```sql
SELECT DISTINCT COUNT(NAME) AS 'Number of Hotels' FROM HOTEL 
```

## What is the average price of a room?
```sql
SELECT AVG(PRICE) AS 'AVERAGE PRICE' FROM ROOM
```

## What is the total revenue per night from all double rooms?
```sql
SELECT SUM(PRICE) AS 'TOTAL REVENUE/PER NIGHT' FROM ROOM WHERE TYPE='SUITE'
```

## How many different guests have made bookings for August?
```sql
SELECT COUNT(HOTEL_NO) AS 'Number of guests in may' FROM BOOKING WHERE DATE_FROM BETWEEN '2022-08-01 00:00:00.000' AND '2022-08-30 00:00:00.000'
```

## List the price and type of all rooms at the Grosvenor Hotel.
```sql
SELECT PRICE, TYPE FROM ROOM WHERE hotel_no IN (SELECT hotel_no FROM HOTEL WHERE NAME = 'Grosvenor Hotel');
```

## List all guests currently staying at the Grosvenor Hotel.
```sql
SELECT guest.name
FROM guest
JOIN booking ON guest.guest_no = booking.guest_no
JOIN hotel ON booking.hotel_no = hotel.hotel_no
WHERE hotel.name = 'Grosvenor Hotel';
```

## List the details of all rooms at the Grosvenor Hotel, including the name of the guest staying in the room, if the room is occupied.
```sql
SELECT r.*, (SELECT g.name FROM guest g WHERE g.guest_no = b.guest_no) AS guest_name
FROM room r
LEFT JOIN booking b ON r.room_no = b.room_no
WHERE r.hotel_no = (SELECT hotel_no FROM hotel WHERE name = 'Grosvenor Hotel');
```

## What is the total income from bookings for the Grosvenor Hotel today?
```sql
SELECT SUM(PRICE) AS 'total income from bookings for the Grosvenor Hotel' FROM ROOM WHERE HOTEL_NO IN (SELECT HOTEL_NO FROM HOTEL WHERE NAME = 'Grosvenor Hotel')
```

## List the rooms that are currently occupied at the Grosvenor Hotel.
```sql
SELECT COUNT(ROOM_NO) AS 'OCCUPIED ROOMS' FROM ROOM WHERE HOTEL_NO IN(SELECT HOTEL_NO FROM HOTEL WHERE NAME = 'Grosvenor Hotel')
```

## What is the total income from unoccupied rooms at the Grosvenor Hotel?
```sql
SELECT SUM(price)
FROM room
JOIN booking ON room.room_no = booking.room_no AND room.hotel_no = booking.hotel_no
WHERE room.hotel_no = 'Grosvenor Hotel' AND booking.date_from <= CURDATE() AND booking.date_to >= CURDATE();
```

## List the number of rooms in each hotel.
```sql
SELECT hotel_no, COUNT(*) AS room_count
FROM room
GROUP BY hotel_no;
```

## List the number of rooms in each hotel in London.
```sql
SELECT h.hotel_no, COUNT(*) AS room_count
FROM room r
JOIN hotel h ON r.hotel_no = h.hotel_no
WHERE h.address = 'London'
GROUP BY h.hotel_no;
```

## What is the average number of bookings for each hotel in August?
```sql
SELECT b.hotel_no, AVG(num_bookings) AS average_bookings
FROM (
    SELECT hotel_no, COUNT(*) AS num_bookings
    FROM booking
    WHERE MONTH(date_from) = 8
    GROUP BY hotel_no, MONTH(date_from)
) AS b
GROUP BY b.hotel_no;
```

## What is the most commonly booked room type for each hotel in London?
```sql
SELECT MAX(TYPE) AS 'most commonly booked room type' FROM ROOM WHERE HOTEL_NO IN (SELECT HOTEL_NO FROM HOTEL WHERE ADDRESS='London')
```

# What is the lost income from unoccupied rooms at each hotel today?
```sql
SELECT r.hotel_no, SUM(r.price) AS lost_income
FROM room r
LEFT JOIN booking b ON r.room_no = b.room_no
WHERE b.room_no IS NULL
GROUP BY r.hotel_no;
```
