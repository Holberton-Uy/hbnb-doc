### Task: Map the Place, Review, and Amenity Entities

#### Objective
Map the `Place`, `Review`, and `Amenity` entities to SQLAlchemy models. This task requires you to apply the knowledge gained from mapping the `User` entity in the previous task to these new entities. You will implement the core database attributes for each entity, and in subsequent tasks, relationships between entities will be defined. No relationships between these entities should be added at this point.

#### Context
In the previous tasks, you mapped the `User` entity and implemented the `UserRepository` to interact with the database through SQLAlchemy. Now, you will extend this mapping to the `Place`, `Review`, and `Amenity` entities. These mappings will prepare the foundation for the relationships that will be added later between places, reviews, amenities, and users.

In this task, you will:
1. Map the core attributes for `Place`, `Review`, and `Amenity` entities.
2. Ensure that each entity has basic CRUD functionality through the repository pattern.
3. Follow the same process you used to map the `User` entity in the previous tasks.

---

#### Instructions

1. **Map the Place, Review, and Amenity Entities**

   - Use SQLAlchemy to map the core attributes of the `Place`, `Review`, and `Amenity` entities.
   - For each entity, define the SQLAlchemy model with the appropriate attributes, making sure to assign primary keys and relevant constraints (e.g., `nullable=False` for required fields).

> [!WARNING]
> Do **not** include relationships between entities yet (these will be added later).
   
   The attributes for each entity and their expected data types are as follows:
   - **Place:**
     - `id` (Integer): Unique identifier (primary key).
     - `title` (String): Title of the place.
     - `description` (String): Description of the place.
     - `price` (Float): Price per night.
     - `latitude` (Float): Latitude of the place.
     - `longitude` (Float): Longitude of the place.
   
   - **Review:**
     - `id` (Integer): Unique identifier (primary key).
     - `text` (String): Text of the review.
     - `rating` (Integer): Rating (from 1 to 5).
   
   - **Amenity:**
     - `id` (Integer): Unique identifier (primary key).
     - `name` (String): Name of the amenity.

2. **Follow the Same Steps as the `User` Entity**
   Just as you mapped the `User` entity in the previous task, apply the same structure for defining models, repositories, and facade methods for `Place`, `Review`, and `Amenity`. These models should now be persisted in the database using SQLAlchemy.

> [!IMPORTANT] 
> You should provide valid entities when calling the API to comply with the Business Logic rules, but they shouldn't be mapped or persisted yet.

3. **Test the Entity Mappings**
   - Initialize the database using `flask shell` and `db.create_all()` after defining your models to create the corresponding tables.
   - Use Postman or cURL to test CRUD operations (Create, Read, Update, Delete) for each entity. This can be done similarly to how you tested the `User` entity.

---

#### Resources
1. **SQLAlchemy Documentation:** [SQLAlchemy](https://docs.sqlalchemy.org/en/20/)
2. **Flask-SQLAlchemy Documentation:** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)
3. **SQLAlchemy Relationship Patterns:** [SQLAlchemy ORM Relationships](https://docs.sqlalchemy.org/en/20/orm/relationships.html)

---

#### Expected Outcome
By the end of this task, you should have mapped the `Place`, `Review`, and `Amenity` entities to SQLAlchemy models, ensuring that their basic attributes are stored in the database. The repositories and facade methods should be updated to handle CRUD operations for each entity. No relationships between entities should be implemented yet; this will be done in a later task.
