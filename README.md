-- # 🎬 Movie Booking Database Design (Simple Solution)

-- ## P1 – Tables and Structure

-- ### Table 1: Movie
-- | movie_id | title | language | format | rating | duration |
-- |----------|---------------------------|----------|--------|--------|----------|
-- | 1 | Dasara | Telugu | 2D | UA | 158 |
-- | 2 | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | UA | 145 |
-- | 3 | Tu Jhoothi Main Makkar | Hindi | 2D | UA | 164 |
-- | 4 | Avatar: The Way of Water | English | 3D | UA | 192 |

-- ### Table 2: Theatre
-- | theatre_id | name | location | city |
-- |------------|------------|-------------|-----------|
-- | 1 | PVR: Nexus | Forum Mall | Bangalore |

-- ### Table 3: Show
-- | show_id | theatre_id | movie_id | show_date | show_time | price | available_seats |
-- |---------|------------|----------|-------------|-----------|-------|-----------------|
-- | 1 | 1 | 1 | 2023-04-25 | 12:15:00 | 300 | 145 |
-- | 2 | 1 | 2 | 2023-04-25 | 13:00:00 | 300 | 150 |
-- | 3 | 1 | 2 | 2023-04-25 | 16:10:00 | 300 | 142 |
-- | 4 | 1 | 2 | 2023-04-25 | 18:20:00 | 300 | 125 |
-- | 5 | 1 | 2 | 2023-04-25 | 19:20:00 | 300 | 138 |
-- | 6 | 1 | 2 | 2023-04-25 | 22:30:00 | 300 | 150 |
-- | 7 | 1 | 3 | 2023-04-25 | 13:15:00 | 300 | 118 |
-- | 8 | 1 | 4 | 2023-04-25 | 13:20:00 | 300 | 195 |

---

-- ## SQL Commands

-- ### Create Tables
CREATE TABLE Movie (
movie_id INT PRIMARY KEY AUTO_INCREMENT,
title VARCHAR(150) NOT NULL,
language VARCHAR(50) NOT NULL,
format VARCHAR(20) NOT NULL,
rating VARCHAR(10),
duration INT
);

CREATE TABLE Theatre (
theatre_id INT PRIMARY KEY AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
location VARCHAR(100) NOT NULL,
city VARCHAR(50) NOT NULL
);

CREATE TABLE Show (
show_id INT PRIMARY KEY AUTO_INCREMENT,
theatre_id INT NOT NULL,
movie_id INT NOT NULL,
show_date DATE NOT NULL,
show_time TIME NOT NULL,
price DECIMAL(8,2) NOT NULL,
available_seats INT DEFAULT 0,
FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id),
FOREIGN KEY (movie_id) REFERENCES Movie(movie_id)
);

-- ### Insert Sample Data
INSERT INTO Movie (title, language, format, rating, duration) VALUES
('Dasara', 'Telugu', '2D', 'UA', 158),
('Kisi Ka Bhai Kisi Ki Jaan', 'Hindi', '2D', 'UA', 145),
('Tu Jhoothi Main Makkar', 'Hindi', '2D', 'UA', 164),
('Avatar: The Way of Water', 'English', '3D', 'UA', 192);

INSERT INTO Theatre (name, location, city) VALUES
('PVR: Nexus', 'Forum Mall', 'Bangalore');

INSERT INTO Show (theatre_id, movie_id, show_date, show_time, price, available_seats) VALUES
(1, 1, '2023-04-25', '12:15:00', 300.00, 145),
(1, 2, '2023-04-25', '13:00:00', 300.00, 150),
(1, 2, '2023-04-25', '16:10:00', 300.00, 142),
(1, 2, '2023-04-25', '18:20:00', 300.00, 125),
(1, 2, '2023-04-25', '19:20:00', 300.00, 138),
(1, 2, '2023-04-25', '22:30:00', 300.00, 150),
(1, 3, '2023-04-25', '13:15:00', 300.00, 118),
(1, 4, '2023-04-25', '13:20:00', 300.00, 195);

-- ## P2 – Query Solution

-- ### Problem
-- List all shows on a given date at a given theatre with show timings.

-- ### Query
SELECT
M.title,
M.language,
M.format,
T.name AS theatre_name,
S.show_date,
TIME_FORMAT(S.show_time, '%h:%i %p') AS show_time,
CONCAT('₹', S.price) AS ticket_price,
S.available_seats
FROM Show S
JOIN Movie M ON S.movie_id = M.movie_id
JOIN Theatre T ON S.theatre_id = T.theatre_id
WHERE S.show_date = '2023-04-25'
AND T.name = 'PVR: Nexus'
ORDER BY S.show_time;

-- ### Expected Output
-- | title | language | format | theatre_name | show_date | show_time | ticket_price | available_seats |
-- |---------------------------|----------|--------|--------------|------------|-----------|--------------|-----------------|
-- | Dasara | Telugu | 2D | PVR: Nexus | 2023-04-25 | 12:15 PM | ₹300 | 145 |
-- | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | PVR: Nexus | 2023-04-25 | 01:00 PM | ₹300 | 150 |
-- | Tu Jhoothi Main Makkar | Hindi | 2D | PVR: Nexus | 2023-04-25 | 01:15 PM | ₹300 | 118 |
-- | Avatar: The Way of Water | English | 3D | PVR: Nexus | 2023-04-25 | 01:20 PM | ₹300 | 195 |
-- | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | PVR: Nexus | 2023-04-25 | 04:10 PM | ₹300 | 142 |
-- | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | PVR: Nexus | 2023-04-25 | 06:20 PM | ₹300 | 125 |
-- | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | PVR: Nexus | 2023-04-25 | 07:20 PM | ₹300 | 138 |
-- | Kisi Ka Bhai Kisi Ki Jaan | Hindi | 2D | PVR: Nexus | 2023-04-25 | 10:30 PM | ₹300 | 150 |
