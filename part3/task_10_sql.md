### Task: SQL Scripts for Table Generation and Initial Data

#### Objective
Create SQL scripts to generate the entire database schema for the HBnB project and populate it with initial data. The scripts should include all tables and relationships necessary to reflect the project’s model and insert the required initial data.

#### Context
In this task, you will focus on designing the database schema using raw SQL to generate tables and insert initial data. This includes creating tables for `User`, `Place`, `Review`, `Amenity`, and their relationships. The purpose of this task is to practice defining databases independently of any ORM, ensuring you understand how to design tables and relationships at the SQL level.

#### Instructions

1. **Generate SQL Scripts for Table Creation**

   Define SQL scripts for each of the following tables:

   - **User Table**:
     - `id`: `CHAR(36) PRIMARY KEY` (UUID format).
     - `first_name`: `VARCHAR(255)`.
     - `last_name`: `VARCHAR(255)`.
     - `email`: `VARCHAR(255) UNIQUE`.
     - `password`: `VARCHAR(255)`.
     - `is_admin`: `BOOLEAN DEFAULT FALSE`.
   
   - **Place Table**:
     - `id`: `CHAR(36) PRIMARY KEY` (UUID format).
     - `title`: `VARCHAR(255)`.
     - `description`: `TEXT`.
     - `price`: `DECIMAL(10, 2)`.
     - `latitude`: `FLOAT`.
     - `longitude`: `FLOAT`.
     - `owner_id`: `CHAR(36)` (Foreign key referencing `User(id)`).

   - **Review Table**:
     - `id`: `CHAR(36) PRIMARY KEY` (UUID format).
     - `text`: `TEXT`.
     - `rating`: `INT` (Between 1 and 5).
     - `user_id`: `CHAR(36)` (Foreign key referencing `User(id)`).
     - `place_id`: `CHAR(36)` (Foreign key referencing `Place(id)`).
     - Add a **unique constraint** on the combination of `user_id` and `place_id` to ensure that a user can only leave one review per place.

   - **Amenity Table**:
     - `id`: `CHAR(36) PRIMARY KEY` (UUID format).
     - `name`: `VARCHAR(255) UNIQUE`.

   - **Place_Amenity Table** (Many-to-Many relationship):
     - `place_id`: `CHAR(36)` (Foreign key referencing `Place(id)`).
     - `amenity_id`: `CHAR(36)` (Foreign key referencing `Amenity(id)`).
     - Add a **composite primary key** for `place_id` and `amenity_id`.

   **Ensure that**:
   - Foreign key constraints are correctly established for relationships.
   - UUIDs are properly generated for the `id` fields.

2. **Insert Initial Data**

   Insert initial data into the database using SQL `INSERT` statements:

   - **Administrator User**:
     - `id`: `36c9050e-ddd3-4c3b-9731-9f487208bbc1` (This ID is fixed).
     - `email`: `admin@hbnb.io`
     - `first_name`: `Admin`
     - `last_name`: `HBnB`
     - `password`: Hash the password `admin1234` using bcrypt2 (use a script or tool to generate the hash).
     - `is_admin`: `True`

   - **Initial Amenities**:
     Insert the following amenities into the `Amenity` table with a randomly generated UUID:
     - WiFi
     - Swimming Pool
     - Air Conditioning

     Generate the `UUID4` values for each `id` using an online UUID generator or a Python script.

3. **Test the SQL Scripts**

   - **Table Creation**: Ensure the tables are created successfully, with all constraints and relationships in place.
   - **Data Insertion**: Verify that the initial data is inserted correctly, ensuring the password is stored in hashed format.
   
   **Test CRUD Operations**:
   - Use `SELECT`, `INSERT`, `UPDATE`, and `DELETE` statements to test data integrity and CRUD functionality for each table.
   - Verify that the admin user is created with `is_admin = TRUE` and that the amenities are inserted correctly.

---

#### Resources
- **SQL Tutorial for Beginners**: [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

#### Expected Outcome
By the end of this task, you should have:
- SQL scripts that generate the full database schema.
- Inserted an administrator user and amenities into the database.
- Tested CRUD operations to verify correct functionality of the schema and data.
