
## Table Creation queries

```mysql
CREATE TABLE User (
  User_ID VARCHAR(36) DEFAULT (UUID()),
  User_name VARCHAR(50) UNIQUE NOT NULL,
  First_name VARCHAR(50) NOT NULL,
  Last_name VARCHAR(50) NOT NULL,
  Date_of_birth DATE NOT NULL,
  Country VARCHAR(50) NOT NULL,
  NIC_code VARCHAR(20) NOT NULL,
  Gender ENUM('Male', 'Female', 'Other') NOT NULL,
  Email VARCHAR(30) UNIQUE NOT NULL,
  Role ENUM('Admin', 'Member') NOT NULL,
  Password VARCHAR(255) NOT NULL,
  PRIMARY KEY (User_ID)
);

CREATE TABLE Location (
  Location_ID INT AUTO_INCREMENT,
  Location VARCHAR(100) NOT NULL,
  Parent_Location_ID INT,
  PRIMARY KEY (Location_ID),
  FOREIGN KEY (Parent_Location_ID) REFERENCES Location(Location_ID)
  ON DELETE RESTRICT
  ON UPDATE CASCADE
);

CREATE TABLE Airport (
  Airport_code CHAR(3) UNIQUE NOT NULL,
  Airport_name VARCHAR(100) NOT NULL,
  Location_ID INT NOT NULL,
  PRIMARY KEY (Airport_code),
  FOREIGN KEY (Location_ID) REFERENCES Location(Location_ID)
  ON DELETE RESTRICT
  ON UPDATE CASCADE
);

CREATE TABLE Airplane_model (
  Airplane_model_ID INT AUTO_INCREMENT,
  Model_name VARCHAR(50) UNIQUE NOT NULL,
  No_of_Economic_Seats INT DEFAULT 0,
  No_of_Business_Seats INT DEFAULT 0,
  No_of_Platinum_Seats INT DEFAULT 0,
  PRIMARY KEY (Airplane_model_ID),
  CONSTRAINT CHK_No_of_Economic_Seats CHECK (No_of_Economic_Seats >= 0),
  CONSTRAINT CHK_No_of_Business_Seats CHECK (No_of_Business_Seats >= 0),
  CONSTRAINT CHK_No_of_Platinum_Seats CHECK (No_of_Platinum_Seats >= 0)
);

CREATE TABLE Airplane (
  Airplane_ID INT AUTO_INCREMENT,
  Airplane_model_ID INT NOT NULL,
  PRIMARY KEY (Airplane_ID),
  FOREIGN KEY (Airplane_model_ID) REFERENCES Airplane_model(Airplane_model_ID)
  ON DELETE RESTRICT
  ON UPDATE CASCADE
);

CREATE TABLE Seat (
  Seat_ID VARCHAR(10),
  Airplane_ID INT NOT NULL,
  Travel_Class ENUM('Economy', 'Business', 'Platinum') NOT NULL,
  PRIMARY KEY (Seat_ID),
  FOREIGN KEY (Airplane_ID) REFERENCES Airplane(Airplane_ID)
  ON DELETE CASCADE
  ON UPDATE CASCADE
);

CREATE TABLE Route (
  Route_ID VARCHAR(10),
  Origin_airport_code CHAR(3) NOT NULL,
  Destination_airport_code CHAR(3) NOT NULL,
  PRIMARY KEY (Route_ID),
  FOREIGN KEY (Origin_airport_code) REFERENCES Airport(Airport_code)
  ON DELETE RESTRICT
  ON UPDATE CASCADE,
  FOREIGN KEY (Destination_airport_code) REFERENCES Airport(Airport_code)
  ON DELETE RESTRICT
  ON UPDATE CASCADE
);

CREATE TABLE Flight (
  Flight_ID VARCHAR(10),
  Airplane_ID INT,
  Route_ID VARCHAR(10),
  Departure_date DATE NOT NULL,
  Arrival_date DATE NOT NULL,
  Arrival_time TIME NOT NULL,
  Departure_time TIME NOT NULL,
  Status ENUM('Scheduled', 'Delayed', 'Cancelled') NOT NULL DEFAULT 'Scheduled',
  PRIMARY KEY (Flight_ID),
  FOREIGN KEY (Airplane_ID) REFERENCES Airplane(Airplane_ID)
  ON DELETE SET NULL
  ON UPDATE CASCADE,
  FOREIGN KEY (Route_ID) REFERENCES Route(Route_ID)
  ON DELETE SET NULL
  ON UPDATE CASCADE,
  CONSTRAINT CHK_Arrival_Date CHECK (Arrival_date >= Departure_date)
);

CREATE TABLE Flight_Pricing (
  Flight_ID VARCHAR(10) NOT NULL,
  Travel_Class ENUM('Economy', 'Business', 'Platinum') NOT NULL,
  Price FLOAT NOT NULL,
  PRIMARY KEY (Flight_ID, Travel_Class),
  FOREIGN KEY (Flight_ID) REFERENCES Flight(Flight_ID)
  ON DELETE CASCADE
  ON UPDATE CASCADE
);

CREATE TABLE Passenger (
  Passenger_ID INT AUTO_INCREMENT,
  Passport_Number VARCHAR(9) UNIQUE NOT NULL,
  Passport_Expire_Date DATE NOT NULL,
  Name VARCHAR(100) NOT NULL,
  Date_of_birth DATE NOT NULL,
  Gender ENUM('Male', 'Female', 'Other') NOT NULL,
  PRIMARY KEY (Passenger_ID)
);

CREATE TABLE Loyalty_detail (
  Membership_Type ENUM('Normal', 'Frequent', 'Gold'),
  Needed_bookings INT NOT NULL,
  Discount DECIMAL(3,2) NOT NULL,
  PRIMARY KEY (Membership_Type),
  CONSTRAINT CHK_Discount_Valid CHECK (Discount >= 0)
);

CREATE TABLE Member_detail (
  User_ID VARCHAR(36),
  No_of_booking INT NOT NULL DEFAULT 0,
  Membership_Type ENUM('Normal', 'Frequent', 'Gold') NOT NULL DEFAULT 'Normal',
  PRIMARY KEY (User_ID),
  FOREIGN KEY (Membership_Type) REFERENCES Loyalty_detail(Membership_Type)
  ON DELETE RESTRICT
  ON UPDATE CASCADE  
);

CREATE TABLE Booking (
  Booking_ID VARCHAR(10),
  Flight_ID VARCHAR(10) NOT NULL,
  User_ID VARCHAR(36),
  Passenger_ID INT NOT NULL,
  Seat_ID VARCHAR(10),
  Issue_date DATETIME DEFAULT NOW(),
  Price FLOAT NOT NULL,
  PRIMARY KEY (Booking_ID),
  FOREIGN KEY (Flight_ID) REFERENCES Flight(Flight_ID)
  ON DELETE RESTRICT
  ON UPDATE CASCADE,
  FOREIGN KEY (User_ID) REFERENCES User(User_ID)
  ON DELETE RESTRICT
  ON UPDATE CASCADE,
  FOREIGN KEY (Passenger_ID) REFERENCES Passenger(Passenger_ID),
  FOREIGN KEY (Seat_ID) REFERENCES Seat(Seat_ID)
  ON DELETE SET NULL
  ON UPDATE CASCADE,
  CONSTRAINT CHK_Price CHECK (Price > 0),
  UNIQUE (Flight_ID, Seat_ID)
);
```

### Indexing

```mysql
CREATE INDEX idx_user_email ON User (Email);

CREATE INDEX idx_member_membership_type ON Member_detail (Membership_Type);

CREATE INDEX idx_flight_status ON Flight (Status);
CREATE INDEX idx_Departure_flight_dates ON Flight (Departure_date);
CREATE INDEX idx_Arrival_flight_dates ON Flight (Arrival_date);
CREATE INDEX idx_flight_pricing_class ON Flight_Pricing (Travel_Class);
CREATE INDEX idx_flight_airplane_id ON Flight (Airplane_ID);
CREATE INDEX idx_flight_route_id ON Flight (Route_ID);

CREATE INDEX idx_booking_flight_id ON Booking (Flight_ID);
CREATE INDEX idx_booking_user_id ON Booking (User_ID);
CREATE INDEX idx_booking_seat ON Booking (Seat_ID);

CREATE INDEX idx_passenger_dob ON Passenger (Date_of_birth);

CREATE INDEX idx_airplane_model_id ON Airplane (Airplane_model_ID);

CREATE INDEX idx_Destination_route_code ON Route (Destination_airport_code);
CREATE INDEX idx_Origin_route_code ON Route (Origin_airport_code);
```



### Triggers


```mysql
DELIMITER //

CREATE TRIGGER after_user_insert
AFTER INSERT ON User
FOR EACH ROW
BEGIN
    -- Check if the role is 'Member'
    IF NEW.Role = 'Member' THEN
        -- Insert into Member_detail with default attributes
        INSERT INTO Member_detail (User_ID, No_of_booking, Membership_Type)
        VALUES (NEW.User_ID, 0, 'Normal');
    END IF;
END //

DELIMITER ;
```



```mysql

-- Insert Loyalty Details
INSERT INTO Loyalty_detail (Membership_Type, Needed_bookings, Discount)
VALUES
('Normal', 0, 0.00),
('Frequent', 10, 0.10),
('Gold', 20, 0.20);

-- Insert Users
INSERT INTO User (User_name, First_name, Last_name, Date_of_birth, Country, NIC_code, Gender, Email, Role, Password)
VALUES 
('JohnDoe', 'John', 'Doe', '1985-05-20', 'USA', '123456789V', 'Male', 'johndoe@example.com', 'Member', 'password123'),
('JaneSmith', 'Jane', 'Smith', '1990-08-15', 'USA', '987654321V', 'Female', 'janesmith@example.com', 'Member', 'password456'),
('AliceJohnson', 'Alice', 'Johnson', '1995-12-01', 'USA', '112233445V', 'Female', 'alicej@example.com', 'Member', 'password789'),
('BobBrown', 'Bob', 'Brown', '1978-11-30', 'USA', '556677889V', 'Male', 'bobbrown@example.com', 'Member', 'password321'),
('CharlieGreen', 'Charlie', 'Green', '2000-03-14', 'USA', '667788990V', 'Other', 'charliegreen@example.com', 'Member', 'password654');

-- Insert Locations
INSERT INTO Location (Location, Parent_Location_ID)
VALUES
('Indonesia', NULL),
('Sri Lanka', NULL),
('India', NULL),
('New Delhi', NULL),
('Singapore', NULL),
('Thailand', NULL),
('Jakarta', 1),
('Bali', 1),
('Denpasar', 8),
('Western', 2),
('Colombo', 10),
('Southern', 2),
('Hambantota', 12),
('Delhi', 3),
('New Delhi', 14),
('Maharashtra', 3),
('Mumbai', 16),
('Tamil Nadu', 3),
('Chennai', 18);

-- Insert Airports
INSERT INTO Airport (Airport_code, Airport_name, Location_ID)
VALUES
('CGK', 'Soekarno-Hatta International Airport', 7),
('DPS', 'Ngurah Rai International Airport', 9),
('BIA', 'Bandaranaike International Airport', 11),
('HRI', 'Mattala Rajapaksa International Airport', 13),
('DEL', 'Indira Gandhi International Airport', 15),
('BOM', 'Chhatrapati Shivaji Maharaj International Airport', 17),
('MAA', 'Chennai International Airport', 19);

-- Insert Airplane Models
INSERT INTO Airplane_model (Model_name, No_of_Economic_Seats, No_of_Business_Seats, No_of_Platinum_Seats)
VALUES
('Boeing 737', 150, 20, 10),
('Boeing 757', 200, 30, 20),
('Airbus A380', 300, 50, 150);

-- Insert Airplanes
INSERT INTO Airplane (Airplane_model_ID)
VALUES
(1), (1), (2), (2), (3);

-- Insert Seats
INSERT INTO Seat (Seat_ID, Airplane_ID, Travel_Class)
VALUES
('ST001', 1, 'Economy'),
('ST002', 2, 'Business'),
('ST003', 3, 'Platinum'),
('ST004', 4, 'Economy'),
('ST005', 5, 'Business');

-- Insert Routes
INSERT INTO Route (Route_ID, Origin_airport_code, Destination_airport_code)
VALUES
('RT001', 'CGK', 'DPS'),
('RT002', 'BIA', 'BOM'),
('RT003', 'HRI', 'CGK'),
('RT004', 'DEL', 'DPS'),
('RT005', 'BOM', 'MAA');

-- Insert Flights
INSERT INTO Flight (Flight_ID, Airplane_ID, Route_ID, Departure_date, Arrival_date, Departure_time, Arrival_time, Status)
VALUES
('FL001', 1, 'RT001', '2024-09-01', '2024-09-01', '08:00:00', '10:00:00', 'Scheduled'),
('FL002', 2, 'RT002', '2024-09-01', '2024-09-01', '12:00:00', '14:30:00', 'Scheduled'),
('FL003', 3, 'RT003', '2024-09-01', '2024-09-01', '15:00:00', '17:00:00', 'Delayed'),
('FL004', 4, 'RT004', '2024-09-01', '2024-09-01', '18:00:00', '20:30:00', 'Scheduled'),
('FL005', 5, 'RT005', '2024-09-01', '2024-09-01', '09:00:00', '11:00:00', 'Cancelled');

-- Insert Flight Pricing
INSERT INTO Flight_Pricing (Flight_ID, Travel_Class, Price)
VALUES
('FL001', 'Economy', 450.00),
('FL002', 'Business', 1200.00),
('FL003', 'Platinum', 2000.00),
('FL004', 'Economy', 600.00),
('FL005', 'Business', 1500.00);

-- Insert Passengers
INSERT INTO Passenger (Passenger_ID, Passport_Number, Passport_Expire_Date, Name, Date_of_birth, Gender)
VALUES
(1, 'A12345678', '2030-12-01', 'John Doe Jr.', '2014-05-20', 'Male'),
(2, 'B98765432', '2031-08-15', 'Jane Smith', '1990-08-15', 'Female'),
(3, 'C85296374', '2032-12-01', 'Alice Johnson', '1995-12-01', 'Female'),
(4, 'D15975385', '2033-11-30', 'Bob Brown Jr.', '2019-11-30', 'Male'),
(5, 'E96325874', '2035-03-14', 'Charlie Green', '2000-03-14', 'Other');
```


### Procedures

```mysql
DELIMITER $$

CREATE PROCEDURE GetAvailableFlights(
    IN p_Origin_airport_code VARCHAR(3),
    IN p_Destination_airport_code VARCHAR(3),
    IN p_Departure_date DATE,
    OUT p_Result VARCHAR(255)
)
BEGIN
    -- Select all flights between two airports on the specified date
    SELECT Flight_ID, Departure_time, Arrival_time 
    FROM Flight
    WHERE Route_ID IN (
        SELECT Route_ID 
        FROM Route 
        WHERE Origin_airport_code = p_Origin_airport_code
        AND Destination_airport_code = p_Destination_airport_code)
    AND Departure_date = p_Departure_date;

    SET p_Result = 'Flights found';
END$$

DELIMITER ;
```

