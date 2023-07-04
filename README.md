# Hotel-Data-Analysis-SQL-Queries
<div style="background-color: #F0F0F0;">
This is the tables that I have created.
NOTE: You can change the values and create your own table.

```sql
SELECT * FROM HOTEL
```

![Screenshot (661)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/550549ca-95a9-4dab-9a1b-f55f035a434f)

Fig 1: HOTEL TABLE


```sql
SELECT * FROM ROOM
```

![Screenshot (662)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/745ee18d-9311-401b-9760-1c973f5aac28)

Fig 2: ROOM TABLE


```sql
SELECT * FROM GUEST
```

![Screenshot (663)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/fd3fdffe-7a05-41ca-b4b5-5dc113a4533e)

Fig 3: GUEST TABLE


```sql
SELECT * FROM BOOKING
```

![Screenshot (664)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/4a46cab5-98b5-45e8-b0b5-ee465dedd496)

Fig 4: BOOKING TABLE


### List full details of all hotels.
```sql
SELECT * FROM HOTEL
```

![Screenshot (684)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/6928985e-c062-40c5-b2fd-045c809c04c2)

Fig 5: Details of all hotels


### List full details of all hotels in London.
```sql
SELECT * FROM HOTEL WHERE ADDRESS='London'
```

![Screenshot (666)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/e75957cf-bdd4-4e74-8a0c-624bf3c896f0)

Fig 6: Details of all hotels in London


### List the names and addresses of all guests in London, alphabetically ordered by name.
```sql
SELECT NAME,ADDRESS FROM GUEST WHERE guest_no IN (
SELECT guest_no FROM BOOKING WHERE hotel_no IN (
SELECT hotel_no FROM HOTEL WHERE ADDRESS='London'))ORDER BY name;
```

![Screenshot (667)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/d444d3be-8900-4e74-a37a-8ac0b7b7b186)

Fig 7: Names and addresses of all guests in London (order by name)


### List all double or family rooms with a price below £40.00 per night, in ascending order of price.
```sql
SELECT * FROM room WHERE (type = 'Suite' OR type = 'Deluxe') AND price < 40.00 ORDER BY price ASC;
```

![Screenshot (668)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/d5ef338e-eb0d-4b72-b2c3-e18489d30590)

Fig 8: Double or family rooms with a price below £40.00 per night (order by price)


### List the bookings for which no date_to has been specified.
```sql
SELECT * FROM BOOKING WHERE date_to IS NULL
```

![Screenshot (669)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/41145aee-e42a-4289-93ff-dfb029b05b8c)

Fig 9: Bookings for which no date_to has been specified


### How many hotels are there?
```sql
SELECT DISTINCT COUNT(NAME) AS 'Number of Hotels' FROM HOTEL 
```

![Screenshot (670)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/8cc03b6e-3a85-42b2-b98c-b9f1c41db663)

Fig 10: Total number of hotels


### What is the average price of a room?
```sql
SELECT AVG(PRICE) AS 'AVERAGE PRICE' FROM ROOM
```

![Screenshot (671)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/26edade0-55ae-45f6-894d-8eaed7ce47d8)

Fig 11: Average price of room


### What is the total revenue per night for all double rooms?
```sql
SELECT SUM(PRICE) AS 'TOTAL REVENUE/PER NIGHT' FROM ROOM WHERE TYPE='SUITE'
```

![Screenshot (672)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/445013d3-986b-4716-b161-6f744214ed1b)

Fig 12: Total revenue per night for all the double rooms


### How many different guests have made bookings for August?
```sql
SELECT COUNT(HOTEL_NO) AS 'Number of guests in may' FROM BOOKING WHERE DATE_FROM BETWEEN '2022-08-01 00:00:00.000' AND '2022-08-30 00:00:00.000'
```

![Screenshot (673)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/8665f34f-5514-4df7-930f-d1f8cc2e8e71)

Fig 13: Different guests have made bookings for August


### List the price and type of all rooms at the Grosvenor Hotel.
```sql
SELECT PRICE, TYPE FROM ROOM WHERE hotel_no IN (SELECT hotel_no FROM HOTEL WHERE NAME = 'Grosvenor Hotel');
```

![Screenshot (674)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/99460a83-85b2-40a0-aaff-75aea47e2695)

Fig 14: Price and type of all rooms at the Grosvenor Hotel


### List all guests currently staying at the Grosvenor Hotel.
```sql
SELECT guest.name
FROM guest
JOIN booking ON guest.guest_no = booking.guest_no
JOIN hotel ON booking.hotel_no = hotel.hotel_no
WHERE hotel.name = 'Grosvenor Hotel';
```

![Screenshot (675)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/9d1e20b5-173b-456d-a3b4-92370f2af6c8)

Fig 15: All guests currently staying at the Grosvenor Hotel


### List the details of all rooms at the Grosvenor Hotel, including the name of the guest staying in the room, if the room is occupied.
```sql
SELECT r.*, (SELECT g.name FROM guest g WHERE g.guest_no = b.guest_no) AS guest_name
FROM room r
LEFT JOIN booking b ON r.room_no = b.room_no
WHERE r.hotel_no = (SELECT hotel_no FROM hotel WHERE name = 'Grosvenor Hotel');
```

![Uploading Screenshot (684).png…]()

Fig 16: Details of all rooms at the Grosvenor Hotel


### What is the total income from bookings for the Grosvenor Hotel today?
```sql
SELECT SUM(PRICE) AS 'total income from bookings for the Grosvenor Hotel' FROM ROOM WHERE HOTEL_NO IN (SELECT HOTEL_NO FROM HOTEL WHERE NAME = 'Grosvenor Hotel')
```

![Screenshot (676)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/0ec83f67-9d27-4bbb-bf78-2c45f55ea5a8)

Fig 17: Total income from bookings for the Grosvenor Hotel today (current day)


### List the rooms that are currently occupied at the Grosvenor Hotel.
```sql
SELECT COUNT(ROOM_NO) AS 'OCCUPIED ROOMS' FROM ROOM WHERE HOTEL_NO IN(SELECT HOTEL_NO FROM HOTEL WHERE NAME = 'Grosvenor Hotel')
```

![Screenshot (677)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/4e0642f6-5f9e-4d02-b449-880a05b730ee)

Fig 18: Rooms that are currently occupied at the Grosvenor Hotel


### What is the total income from unoccupied rooms at the Grosvenor Hotel?
```sql
SELECT SUM(price)
FROM room
JOIN booking ON room.room_no = booking.room_no AND room.hotel_no = booking.hotel_no
WHERE room.hotel_no = 'Grosvenor Hotel' AND booking.date_from <= CURDATE() AND booking.date_to >= CURDATE();
```

![Screenshot (686)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/3be77060-edbf-4c51-9ebb-94d65baa6cce)

Fig 19: Total income from unoccupied rooms at the Grosvenor Hotel


### List the number of rooms in each hotel.
```sql
SELECT hotel_no, COUNT(*) AS room_count
FROM room
GROUP BY hotel_no;
```

![Screenshot (679)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/8ed1022c-b7c3-4510-a0f3-932eb91be311)

Fig 20: Number of rooms in each hotel


### List the number of rooms in each hotel in London.
```sql
SELECT h.hotel_no, COUNT(*) AS room_count
FROM room r
JOIN hotel h ON r.hotel_no = h.hotel_no
WHERE h.address = 'London'
GROUP BY h.hotel_no;
```

![Screenshot (680)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/403132f9-98e7-4ad9-b758-cb12888dd09a)

Fig 21: Number of rooms in each hotel in London


### What is the average number of bookings for each hotel in August?
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

![Screenshot (681)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/fe2c57f6-25fb-4a58-8f81-f6f33e6ed797)

Fig 22: Average number of bookings for each hotel in August


### What is the most commonly booked room type for each hotel in London?
```sql
SELECT MAX(TYPE) AS 'most commonly booked room type' FROM ROOM WHERE HOTEL_NO IN (SELECT HOTEL_NO FROM HOTEL WHERE ADDRESS='London')
```

![Screenshot (682)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/b1d8b0ed-3ddc-4474-956b-bb1735c719a5)

Fig 23: Most commonly booked room type for each hotel in London


### What is the lost income from unoccupied rooms at each hotel today?
```sql
SELECT r.hotel_no, SUM(r.price) AS lost_income
FROM room r
LEFT JOIN booking b ON r.room_no = b.room_no
WHERE b.room_no IS NULL
GROUP BY r.hotel_no;
```

![Screenshot (687)](https://github.com/khushimdave/Hotel-Data-Analysis-SQL-Queries/assets/94516006/8b17521f-0c3b-4af4-8a95-ae575d4d3126)

Fig 24: Lost income from unoccupied rooms at each hotel today (current day)

</div>
