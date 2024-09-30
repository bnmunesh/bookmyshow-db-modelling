# BookMyShow-like Database Schema

This repository contains the SQL schema and sample data for a movie ticket booking system similar to BookMyShow. The database is designed to manage movies, theatres, shows, and bookings.

## Contents

The following SQL script creates the database schema, inserts sample data, and includes an example query:

```sql
use bookmyshow;

-- Table Movies
CREATE TABLE Movies (
    movie_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    genre VARCHAR(100),
    movie_length INT UNSIGNED NOT NULL,
    language VARCHAR(50) NOT NULL,
    release_date DATE NOT NULL,
    rating DECIMAL(3,2)
);

-- Table Cities
CREATE TABLE Cities (
    city_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL
);

-- Table Theatres
CREATE TABLE Theatres (
    theatre_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    theatre_name VARCHAR(255) NOT NULL,
    city_id BIGINT NOT NULL,
    address VARCHAR(255),
    FOREIGN KEY (city_id) REFERENCES Cities(city_id)
);

-- Table Screens
CREATE TABLE Screens (
    screen_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    screen_name VARCHAR(255) NOT NULL,
    theatre_id BIGINT NOT NULL,
    total_seats INT UNSIGNED NOT NULL,
    FOREIGN KEY (theatre_id) REFERENCES Theatres(theatre_id)
);

-- Table Shows
CREATE TABLE Shows (
    show_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    movie_id BIGINT NOT NULL,
    theatre_id BIGINT NOT NULL,
    screen_id BIGINT NOT NULL,
    show_datetime DATETIME NOT NULL,
    FOREIGN KEY (movie_id) REFERENCES Movies(movie_id),
    FOREIGN KEY (theatre_id) REFERENCES Theatres(theatre_id),
    FOREIGN KEY (screen_id) REFERENCES Screens(screen_id),
    UNIQUE(screen_id, show_datetime)
);

-- Table Users
CREATE TABLE Users (
    user_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255),
    email VARCHAR(255) NOT NULL UNIQUE,
    phone_number VARCHAR(50) NOT NULL UNIQUE,
    hashed_password VARCHAR(255) NOT NULL
);

-- Table Bookings
CREATE TABLE Bookings (
    booking_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_id BIGINT NOT NULL,
    show_id BIGINT NOT NULL,
    booked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
    seat_number VARCHAR(50) NOT NULL,
    FOREIGN KEY (user_id) REFERENCES Users(user_id),
    FOREIGN KEY (show_id) REFERENCES Shows(show_id),
    UNIQUE(show_id, seat_number)
);


-- Insert data into Cities
INSERT INTO Cities (name) VALUES ('New York');

-- Insert data into Theatres
INSERT INTO Theatres (theatre_name, city_id, address) VALUES 
('Cinema Palace', 1, '123 Main St, New York, NY'),
('Film Hub', 1, '456 Elm St, New York, NY');

-- Insert data into Movies
INSERT INTO Movies (title, genre, movie_length, language, release_date, rating) VALUES 
('The Grand Adventure', 'Action', 120, 'English', '2023-06-15', 8.5),
('Love in the City', 'Romance', 90, 'English', '2023-07-01', 7.0),
('Mystery at Midnight', 'Thriller', 100, 'English', '2023-08-10', 6.5),
('The Lost Treasure', 'Adventure', 130, 'English', '2023-05-20', 9.0),
('Comedy Nights', 'Comedy', 95, 'English', '2023-09-05', 7.5);

-- Insert data into Screens
INSERT INTO Screens (screen_name, theatre_id, total_seats) VALUES 
('Screen 1', 1, 150),
('Screen 2', 1, 120),
('Screen 1', 2, 200),
('Screen 2', 2, 180);

-- Insert data into Shows
INSERT INTO Shows (movie_id, theatre_id, screen_id, show_datetime) VALUES 
(1, 1, 1, '2024-10-01 14:00:00'),
(1, 1, 2, '2024-10-01 18:00:00'),
(2, 1, 1, '2024-10-02 15:00:00'),
(3, 1, 2, '2024-10-02 20:00:00'),
(4, 2, 1, '2024-10-03 16:00:00'),
(5, 2, 2, '2024-10-03 19:30:00');

-- Insert data into Users
INSERT INTO Users (first_name, last_name, email, phone_number, hashed_password) VALUES 
('John', 'Doe', 'john.doe@example.com', '123-456-7890', 'hashedpassword1'),
('Jane', 'Smith', 'jane.smith@example.com', '098-765-4321', 'hashedpassword2'),
('Alice', 'Johnson', 'alice.johnson@example.com', '555-555-5555', 'hashedpassword3');

-- Insert data into Bookings
INSERT INTO Bookings (user_id, show_id, seat_number) VALUES 
(1, 1, 'A1'),
(2, 3, 'B2'),
(3, 5, 'C3'),
(1, 2, 'A2'),
(2, 4, 'B4');


-- Show table data
select * from Cities;
select * from Theatres;
select * from Movies;
select * from Screens;
select * from Shows;
select * from Users;
select * from Bookings;


--  Query to list down all the shows on a given date at a given theatre along with their respective show timings.
SELECT 
    m.title AS movie_title,
    s.screen_name,
    sh.show_datetime
FROM 
    Shows sh
JOIN 
    Movies m ON sh.movie_id = m.movie_id
JOIN 
    Screens s ON sh.screen_id = s.screen_id
JOIN 
    Theatres t ON sh.theatre_id = t.theatre_id
WHERE 
    t.theatre_name = 'Cinema Palace' 
    AND DATE(sh.show_datetime) = '2024-10-01'
ORDER BY 
    sh.show_datetime;
```

## Setup Instructions

1. Ensure you have MySQL installed on your system.
2. Create a new database named `bookmyshow`.
3. Copy and paste the entire SQL script provided above into your MySQL client or command-line interface.
4. Execute the script to create the tables, insert sample data, and run the example query.
5. You can now run additional queries against the database to retrieve or manipulate data as needed.

Feel free to modify the schema or add more data as needed for your specific use case.
