### Task 7: Map the User Entity to SQLAlchemy Model

#### Objective
Map the `User` entity to a SQLAlchemy model, ensuring the correct database relationships, attribute definitions, and CRUD operations are implemented. You will incorporate the ORM functionality within the repository layer, service layer (Facade), and API layer for full integration. The password hashing mechanism from the previous task should remain functional.

#### Context
In the previous task, the repository was updated to use SQLAlchemy for database persistence, replacing the in-memory repository. This task extends that functionality by mapping the `User` entity to the database using SQLAlchemy. The new `UserRepository` will be responsible for handling user-specific queries, enhancing the flexibility and maintainability of the application.

In this task, you will:
1. Map the `User` entity to a SQLAlchemy model, including attributes like `first_name`, `last_name`, `email`, `password`, and `is_admin`.
2. Implement the `UserRepository` class to interact with the database using SQLAlchemy.
3. Refactor the `Facade` to use the `UserRepository` for user-related operations.

---

#### Instructions

1. **Map the User Entity**

   In the `models/user.py` file, define the `User` class as a SQLAlchemy model:
   - Each attribute (e.g., `first_name`, `last_name`, `email`, `password`, `is_admin`) should map to a column in the database.
   - Add relevant constraints such as `unique=True` for the `email` column to ensure no duplicate emails are allowed.
   
   **Example:**
   ```python
   from app import db
   from flask_bcrypt import Bcrypt
   import uuid
   
   bcrypt = Bcrypt()
   
   class User(db.Model):
       __tablename__ = 'users'
   
       id = db.Column(db.String(36), primary_key=True, default=lambda: str(uuid.uuid4()))
       first_name = db.Column(db.String(50), nullable=False)
       last_name = db.Column(db.String(50), nullable=False)
       email = db.Column(db.String(120), nullable=False, unique=True)
       password = db.Column(db.String(128), nullable=False)
       is_admin = db.Column(db.Boolean, default=False)
   
       def hash_password(self, password):
           """Hash the password before storing it."""
           self.password = bcrypt.generate_password_hash(password).decode('utf-8')
   
       def verify_password(self, password):
           """Verify the hashed password."""
           return bcrypt.check_password_hash(self.password, password)
   ```

   **Explanation of SQLAlchemy Methods:**
   - `db.Column`: Defines a column in the database.
   - `primary_key=True`: Marks the column as the primary key.
   - `unique=True`: Ensures unique values (used here to prevent duplicate emails).


1. **Implement the UserRepository**

   Instead of relying solely on the generic `SQLAlchemyRepository`, create a specific `UserRepository` class that extends from the base repository.

   Creating a specific `UserRepository` class offers several key benefits:
  
   1. **Entity-Specific Behavior**: While `SQLAlchemyRepository` provides the basic CRUD operations, different entities often require unique operations or queries that are specific to their domain. For instance, the `UserRepository` might need to implement operations like finding a user by email or checking for password reset conditions, which are unique to the user entity. A custom `UserRepository` can encapsulate these entity-specific behaviors, making the code cleaner and more maintainable. 
   2. **Encapsulation of Business Logic**: A specific `UserRepository` helps encapsulate user-related logic that interacts with the database in one place. It allows for better code organization by keeping `User`-related queries in their own repository, thus avoiding bloating the generic repository with complex user-specific queries.
   3. **Extensibility**: If new features or more complex logic are needed for the `User` entity (like authentication, logging, or role management), a dedicated repository class makes it easier to extend functionality in the future. This structure allows for better scaling and customization without cluttering the core repository.
   4. **Separation of Concerns**: A specialized `UserRepository` maintains the repository pattern’s principle of separating data access logic from the business logic. This keeps the business layer agnostic to changes in how the data is persisted or queried while still allowing user-specific operations to remain isolated.
  
   By creating a dedicated `UserRepository`, you achieve a more focused, maintainable, and extendable architecture, especially when working with entities that require specific behaviors beyond generic CRUD operations.

   **Example:**
   ```python
   from app.models.user import User
   from app import db
   from app.persistence.repository import SQLAlchemyRepository

   class UserRepository(SQLAlchemyRepository):
       def __init__(self):
           super().__init__(User)

       def get_user_by_email(self, email):
           return self.model.query.filter_by(email=email).first()
   ```

   **Explanation:**
   - `get_user_by_email`: This method allows you to search for a user by email, something specific to the `User` entity. This kind of custom behavior is what makes having a dedicated `UserRepository` useful over the generic repository.

4. **Refactor the Facade to Use the UserRepository**

   Update the `HBnBFacade` class to use the newly created `UserRepository` for all user-related operations, ensuring that the facade communicates with the database through the repository layer.

   **Example:**
   ```python
   from app.services.repositories.user_repository import UserRepository

   class HBnBFacade:
       def __init__(self):
           self.user_repo = UserRepository()

       def create_user(self, user_data):
           user = User(**user_data)
           user.hash_password(user_data['password'])
           self.user_repo.add(user)
           return user

       def get_user(self, user_id):
           return self.user_repo.get(user_id)

       def get_user_by_email(self, email):
           return self.user_repo.get_user_by_email(email)
   ```

   **Explanation:**
   - `create_user`: When creating a user, the password is hashed before saving the user in the database.
   - `get_user_by_email`: This method uses the `UserRepository`'s `get_user_by_email` function to retrieve users by email.

5. **Initialize the Database and Test the Integration**

   Now that the `User` model is defined and the repository is set up, you need to initialize the database to create the `users` table.

   - To initialize the database and create the table, run:
     ```bash
     flask shell
     >>> from app import db
     >>> db.create_all()
     ```

   - Test user creation and retrieval using Postman or cURL. For example, create a new user:
     ```bash
     curl -X POST "http://127.0.0.1:5000/api/v1/users/" -H "Content-Type: application/json" -d '{
       "first_name": "John",
       "last_name": "Doe",
       "email": "john.doe@example.com",
       "password": "password123"
     }'
     ```

   - Then retrieve the user by ID:
     ```bash
     curl -X GET "http://127.0.0.1:5000/api/v1/users/<user_id>"
     ```

---

#### Resources
1. **SQLAlchemy Documentation:** [SQLAlchemy](https://docs.sqlalchemy.org/en/14/)
2. **Flask-SQLAlchemy Documentation:** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)
3. **Flask-Bcrypt Documentation:** [Flask-Bcrypt](https://flask-bcrypt.readthedocs.io/en/latest/)
4. **Testing REST APIs with cURL:** [Everything cURL](https://everything.curl.dev/)

#### Expected Outcome
By the end of this task, the `User` entity will be fully mapped to a SQLAlchemy model, and the repository pattern will be updated to interact with the database using SQLAlchemy. You will have refactored the `Facade` to use the new `UserRepository`, and the application will now store user data in the database persistently.
