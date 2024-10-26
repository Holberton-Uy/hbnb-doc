### Task: Implement SQLAlchemy Repository

#### Objective
Replace the in-memory repository with a SQLAlchemy-based repository for persistence. In this task, you will create the `SQLAlchemyRepository` and integrate it into the project for managing database interactions. This task will lay the foundation for further model mapping and database setup in subsequent tasks.

#### Context
In the previous parts of the project, the persistence layer was managed using an in-memory repository. This task introduces SQLAlchemy to persist data in an SQLite database during development, preparing the application for a production-ready relational database. The repository pattern remains the same, but the implementation will now interact with SQLAlchemy for all CRUD operations. Due to the fact that the database has not yet been initialized, this task focuses only on creating the repository. Model mapping and database initialization will follow in the next task.

You will:
1. Create the SQLAlchemy repository that implements the existing repository interface.
2. Refactor the existing Facade to utilize the SQLAlchemy-based repository for user operations.
3. Provide code and detailed instructions for integration, but no database initialization will be performed yet.

> [!IMPORTANT]
> Read this guide before start:  [Transitioning from In-Memory to Database Persistence: A Step-by-Step Guide](./concept-memory_to_db.md)

---

#### Instructions

1. **Set Up SQLAlchemy in the Project**

   - Add the necessary dependencies for SQLAlchemy to your `requirements.txt`:
     ```txt
     sqlalchemy
     flask-sqlalchemy
     ```

   - Initialize SQLAlchemy in `app/__init__.py`, but note that database initialization (e.g., table creation) will happen in the next task:
     ```python
     from flask import Flask
     from flask_sqlalchemy import SQLAlchemy

     db = SQLAlchemy()

     def create_app(config_class=config.DevelopmentConfig):
         #
         # Existent code with app Flask instance
         #
         db.init_app(app)

         # ...
     ```

2. **Add configuration parameters for SQLAlchemy**

   - Add the following configuration keys to the `DevelopmentConfig` Class in the `config.py` file
     ```python
     import os
 
     class Config:
         SECRET_KEY = os.getenv('SECRET_KEY', 'default_secret_key')
         DEBUG = False
 
     class DevelopmentConfig(Config):
         DEBUG = True
         SQLALCHEMY_DATABASE_URI = 'sqlite:///development.db'
         SQLALCHEMY_TRACK_MODIFICATIONS = False
 
     config = {
         'development': DevelopmentConfig,
         'default': DevelopmentConfig
     }
     ```

   - `SQLALCHEMY_DATABASE_URI`: Sets the URI for the SQLite database.
   - `SQLALCHEMY_TRACK_MODIFICATIONS`: Disables tracking of object modifications, which is a feature that can consume additional memory.     

2. **Create the SQLAlchemy Repository**

   - The `SQLAlchemyRepository` class will implement the `Repository` interface from **app/persistence/repository.py**, which was provided in the earlier tasks of the project.
   - The repository will handle database CRUD operations using SQLAlchemy, adhering to the same structure as the in-memory repository.

   ```python
   from app import db

   class SQLAlchemyRepository(Repository):
       def __init__(self, model):
           self.model = model

       def add(self, obj):
           db.session.add(obj)
           db.session.commit()

       def get(self, obj_id):
           return self.model.query.get(obj_id)

       def get_all(self):
           return self.model.query.all()

       def update(self, obj_id, data):
           obj = self.get(obj_id)
           if obj:
               for key, value in data.items():
                   setattr(obj, key, value)
               db.session.commit()

       def delete(self, obj_id):
           obj = self.get(obj_id)
           if obj:
               db.session.delete(obj)
               db.session.commit()

       def get_by_attribute(self, attr_name, attr_value):
           return self.model.query.filter_by(**{attr_name: attr_value}).first()
   ```

   - This `SQLAlchemyRepository` will be flexible and reusable for different entities in the project (e.g., `User`, `Place`, `Review`).

3. **Refactor the Facade**

   - The Facade class should now rely on the `SQLAlchemyRepository` for interacting with the database. Since no model mapping or database initialization has occurred yet, only the structure will be set up for now.
   - Refactor the `HBnBFacade` in `app/services/facade.py` to replace the in-memory repository with the SQLAlchemy-based repository:

     ```python
     from app.models.user import User
     from app.persistence.repository import SQLAlchemyRepository

     class HBnBFacade:
         def __init__(self):
             self.user_repo = SQLAlchemyRepository(User)

         def create_user(self, user_data):
             user = User(**user_data)
             self.user_repo.add(user)
             return user

         def get_user(self, user_id):
             return self.user_repo.get(user_id)
     ```

4. **Hold Off on Database Testing**
   - **Important:** Since the models have not been mapped yet, you will not be able to fully test or initialize the database at this stage. In the next task, the `User` model will be mapped to the database, at which point you can create tables and initialize the database schema.
   - The testing and verification of the persistence layer will be covered once the model mapping is complete.

---

#### Resources

- **SQLAlchemy Documentation:** [SQLAlchemy](https://docs.sqlalchemy.org/en/20/)
- **Flask-SQLAlchemy Documentation:** [Flask-SQLAlchemy](https://flask-sqlalchemy.palletsprojects.com/en/2.x/)

---

#### Expected Outcome
By the end of this task, you will have implemented the `SQLAlchemyRepository` and refactored the Facade to use this new repository for data persistence. However, since the model mapping and database initialization will be performed in the next task, the actual integration testing will be delayed until the next step.
