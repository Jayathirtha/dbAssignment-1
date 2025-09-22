# dbAssignment-1
Problem Solving Case - BookMyShow

## P1 : 

### Entities and Attributes

1) **Movie**	MovieID (PK), Title, Language, Genre, Duration, Rating (e.g., UA), ReleaseDate	Information about the film itself.
2) **Theatre**	TheatreID (PK), Name, Location/City, Address	Information about the venue.
3) **Screen**	ScreenID (PK), TheatreID (FK), ScreenName, Capacity, Technology (e.g., IMAX, Dolby)	A specific screen or auditorium within a theatre.
4) **Shows**	ShowID (PK), MovieID (FK), ScreenID (FK), ShowDateTime (Unique/Index), ShowDate, ShowTime	The schedule for a movie on a specific screen.
5) **ShowFormat**	FormatID (PK), FormatName (e.g., 2D, 3D)	Details about the projection/audio technology for a show.



### Theatre Table 
CREATE TABLE Theatre (
    TheatreID INTEGER PRIMARY KEY,
    Name TEXT NOT NULL,
    Location TEXT NOT NULL,
    Address TEXT
);



### Movie Table
CREATE TABLE Movie (
    MovieID INTEGER PRIMARY KEY,
    Title TEXT NOT NULL,
    Language TEXT NOT NULL,
    Rating TEXT,
    DurationMinutes INTEGER
);



### Screen Table 
CREATE TABLE Screen (
    ScreenID INTEGER PRIMARY KEY,
    TheatreID INTEGER NOT NULL,
    Name TEXT NOT NULL,
    Capacity INTEGER,
    FOREIGN KEY (TheatreID) REFERENCES Theatre(TheatreID)
);

### Show Table

CREATE TABLE Shows (
    ShowID INTEGER PRIMARY KEY,
    MovieID INTEGER NOT NULL,
    ScreenID INTEGER NOT NULL,
    ShowDate DATE NOT NULL,
    ShowTime TIME NOT NULL,
    FormatName TEXT,             
    FOREIGN KEY (MovieID) REFERENCES Movie(MovieID),
    FOREIGN KEY (ScreenID) REFERENCES Screen(ScreenID)
);


### Sample Data for Theatre

INSERT INTO Theatre (TheatreID, Name, Location) VALUES
(1, 'PVR: Nexus ', 'Bengaluru');


### Sample Data for Movie

INSERT INTO Movie (MovieID, Title, Language, Rating, DurationMinutes) VALUES
(101, 'A', 'Kannada', 'U', 150),
(102, 'Dune', 'English', 'UA', 155),
(103, 'Dune: Part Two', 'English', 'UA', 166),
(104, 'Avatar: The Way of Water', 'English', 'UA', 192);


 Sample Data for Screen

INSERT INTO Screen (ScreenID, TheatreID, Name, Capacity) VALUES
(1, 1, 'Screen 1', 200),
(2, 1, 'Screen 2 - ATMOS', 150),
(3, 1, 'Screen 3 - Playhouse', 100);


### Sample Data for Show 

INSERT INTO Shows (ShowID, MovieID, ScreenID, ShowDate, ShowTime, FormatName) VALUES
(1001, 101, 1, '2023-04-25', '12:15:00', '2D'),
(1002, 102, 2, '2023-04-25', '13:00:00', '2D'),
(1003, 102, 2, '2023-04-25', '16:00:00', '2D'),
(1007, 103, 1, '2023-04-25', '13:15:00', '2D'),
(1008, 104, 3, '2023-04-25', '13:10:00', '3D');



### P2- query to list down all the shows on a given date at a given theatre along with their respective show timings


SELECT
    movie.Title,
    movie.Language,
    shows.ShowTime,
    shows.FormatName
FROM
    Shows AS shows
JOIN
    Movie AS movie ON shows.MovieID = movie.MovieID
JOIN
    Screen AS screen ON shows.ScreenID = screen.ScreenID
WHERE
    screen.TheatreID = 1                         
    AND shows.ShowDate = '2023-04-25'
ORDER BY
    movie.Title, shows.ShowTime;
