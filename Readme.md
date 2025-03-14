# Mega City Cab Booking System

Mega City Cab is a computerized system designed for managing customer bookings, car and driver information, and billing for a cab service in Colombo. This application requires a **MySQL** database to store customer information, booking details, and other related data.

## Requirements

- **Java**: Java Development Kit (JDK) 11 or later
- **MySQL**: MySQL 5.7 or later
- **MySQL JDBC Driver**: Required to connect to the MySQL database
- **IDE**: Eclipse, IntelliJ IDEA, or any Java-supported IDE

## Prerequisites

1. **MySQL Database Setup**
   - Install MySQL on your machine if it is not already installed. You can download MySQL from [here](https://dev.mysql.com/downloads/).
   - Create a database called `cab_booking`:

     ```sql
     CREATE DATABASE cab_booking;
     ```

2. **Database Configuration**
   - Once the database is created, run the following SQL commands to create the required tables:

     ```sql
     -- Create 'users' table
     CREATE TABLE users (
         user_id INT AUTO_INCREMENT PRIMARY KEY,
         username VARCHAR(50) UNIQUE NOT NULL,
         password VARCHAR(255) NOT NULL,
         email VARCHAR(100) UNIQUE NOT NULL,
         phone VARCHAR(20),
         full_name VARCHAR(100),
         role VARCHAR(10) CHECK (role IN ('CUSTOMER', 'ADMIN')) NOT NULL,
         status VARCHAR(20) NOT NULL DEFAULT 'ACTIVE'
     );

     -- Create 'vehicles' table
     CREATE TABLE vehicles (
         vehicle_id INT AUTO_INCREMENT PRIMARY KEY,
         driver_name VARCHAR(100) NOT NULL,
         vehicle_number VARCHAR(50) UNIQUE NOT NULL,
         model VARCHAR(100),
         status VARCHAR(20) CHECK (status IN ('AVAILABLE', 'BOOKED', 'IN_MAINTENANCE')) NOT NULL
     );

     -- Create 'bookings' table
     CREATE TABLE bookings (
         booking_id INT AUTO_INCREMENT PRIMARY KEY,
         user_id INT,
         vehicle_id INT,
         pickup_location VARCHAR(255) NOT NULL,
         dropoff_location VARCHAR(255) NOT NULL,
         distance DOUBLE NOT NULL,
         fare DOUBLE NOT NULL,
         status VARCHAR(20) CHECK (status IN ('PENDING', 'CONFIRMED', 'COMPLETED', 'CANCELLED')) NOT NULL,
         booking_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
         payment_method VARCHAR(20),
         transaction_id VARCHAR(100) UNIQUE,
         FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
         FOREIGN KEY (vehicle_id) REFERENCES vehicles(vehicle_id) ON DELETE SET NULL
     );

     -- Create 'payments' table
     CREATE TABLE payments (
         payment_id INT AUTO_INCREMENT PRIMARY KEY,
         booking_id INT,
         payment_method VARCHAR(10) CHECK (payment_method IN ('CASH', 'CARD')) NOT NULL,
         amount DOUBLE NOT NULL,
         payment_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
         FOREIGN KEY (booking_id) REFERENCES bookings(booking_id) ON DELETE CASCADE
     );
     ```

3. **MySQL User Setup**
   - Ensure that the MySQL user (`my_user`) has appropriate privileges to access the `cab_booking` database.
   - You can grant privileges using the following SQL commands:

     ```sql
     GRANT ALL PRIVILEGES ON cab_booking.* TO 'my_user'@'localhost' IDENTIFIED BY 'my_password';
     FLUSH PRIVILEGES;
     ```

4. **MySQL JDBC Driver**
   - You need the MySQL JDBC driver for Java to interact with the MySQL database. Ensure that the `mysql-connector-java` JAR file is added to your project classpath.
   - You can download the driver from [here](https://dev.mysql.com/downloads/connector/j/).

