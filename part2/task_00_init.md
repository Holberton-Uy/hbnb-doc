# Project Setup and Package Initialization

## Context

Before diving into the implementation of the business logic and API endpoints, it's essential to have **a well-organized project structure**. A **clear** and **modular** organization will help maintain the codebase, make it easier to integrate new features, and **ensure that your application is scalable**.

Additionally, to simplify the implementation, you are provided with the **complete in-memory repository code**.

## In this task, you will

1. Set up the structure for the Presentation, Business Logic, and Persistence layers, creating the necessary folders, packages, and files.
2. Prepare the project to use the Facade pattern for communication between layers.
3. Implement the in-memory repository to handle object storage and validation.
4. Plan for future integration of the Persistence layer, even though it won't be fully implemented in this part.

Although the Persistence layer will be fully implemented in Part 3, this task includes the implementation of the in-memory repository. This repository will later be replaced by a database-backed solution in Part 3.

## Instructions

1. **Create the Project Directory Structure**:

    Your project should be organized into the following structure:

    ```text
    hbnb/
    ├── app/
    │   ├── __init__.py
    │   ├── api/
    │   │   ├── __init__.py
    │   │   ├── v1/
    │   │       ├── __init__.py
    │   │       ├── users.py
    │   │       ├── places.py
    │   │       ├── reviews.py
    │   │       ├── amenities.py
    │   ├── models/
    │   │   ├── __init__.py
    │   │   ├── user.py
    │   │   ├── place.py
    │   │   ├── review.py
    │   │   ├── amenity.py
    │   ├── services/
    │   │   ├── __init__.py
    │   │   ├── facade.py
    │   ├── persistence/
    │       ├── __init__.py
    │       ├── repository.py
    ├── run.py
    ├── config.py
    ├── requirements.txt
    ├── README.md
    ```

    **Explanation:**

    - The `app/` directory contains the core application code.
    - The `api/` subdirectory houses the API endpoints, organized by version (`v1/`).
    - The `models/` subdirectory contains the business logic classes (e.g., `user.py`, `place.py`).
    - The `services/` subdirectory is where the Facade pattern is implemented, managing the interaction between layers.
    - The `persistence/` subdirectory is where the in-memory repository is implemented. This will later be replaced by a database-backed solution using SQL Alchemy.
    - `run.py` is the entry point for running the Flask application.
    - `config.py` will be used for configuring environment variables and application settings.
    - `requirements.txt` will list all the Python packages needed for the project.
    - `README.md` will contain a brief overview of the project.

2. **Initialize Python Packages**

    In each directory that is intended to be a Python package (e.g., `app/`, `api/`, `models/`, `services/`, `persistence/`, `v1/`), create an empty `__init__.py` file. This tells Python to treat these directories as importable packages.

3. **Set Up the Flask Application with Placeholders**

    Inside the `app/` directory, create the Flask application instance within the `__init__.py` file:

    ```python
    from flask import Flask
    from flask_restx import Api

    def create_app():
        app = Flask(__name__)
        api = Api(app, version='1.0', title='HBnB API', description='HBnB Application API', doc='/api/v1/')

        # Placeholder for API namespaces (endpoints will be added later)
        # Additional namespaces for places, reviews, and amenities will be added later

        return app
    ```

4. **Implement the In-Memory Repository**

    The in-memory repository will handle object storage and validation. It follows a consistent interface that will later be replaced by a database-backed repository.

    **Create the following structure under the `persistence/` directory:**

    ```text
    hbnb/
    ├── app/
    │   ├── persistence/
    │       ├── __init__.py
    │       ├── repository.py
    ```

    **Inside `repository.py`, the in-memory repository and interface will be fully implemented:**

    ```python
    from abc import ABC, abstractmethod

    class Repository(ABC):
        @abstractmethod
        def add(self, obj):
            pass

        @abstractmethod
        def get(self, obj_id):
            pass

        @abstractmethod
        def get_all(self):
            pass

        @abstractmethod
        def update(self, obj_id, data):
            pass

        @abstractmethod
        def delete(self, obj_id):
            pass

        @abstractmethod
        def get_by_attribute(self, attr_name, attr_value):
            pass


    class InMemoryRepository(Repository):
        def __init__(self):
            self._storage = {}

        def add(self, obj):
            self._storage[obj.id] = obj

        def get(self, obj_id):
            return self._storage.get(obj_id)

        def get_all(self):
            return list(self._storage.values())

        def update(self, obj_id, data):
            obj = self.get(obj_id)
            if obj:
                obj.update(data)

        def delete(self, obj_id):
            if obj_id in self._storage:
                del self._storage[obj_id]

        def get_by_attribute(self, attr_name, attr_value):
            return next((obj for obj in self._storage.values() if getattr(obj, attr_name) == attr_value), None)
    ```

5. **Plan for the Facade Pattern with Placeholders**

    In the `services/` subdirectory, create a `facade.py` file where you will define the `HBnBFacade` class. This class will handle communication between the Presentation, Business Logic, and Persistence layers. You will interact with the repositories (like the in-memory repository) through this Class:

    ```python
    from app.persistence.repository import InMemoryRepository

    class HBnBFacade:
        def __init__(self):
            self.user_repo = InMemoryRepository()
            self.place_repo = InMemoryRepository()
            self.review_repo = InMemoryRepository()
            self.amenity_repo = InMemoryRepository()

        # Placeholder method for creating a user
        def create_user(self, user_data):
            # Logic will be implemented in later tasks
            pass

        # Placeholder method for fetching a place by ID
        def get_place(self, place_id):
            # Logic will be implemented in later tasks
            pass
    ```

    The methods in the Facade use placeholders to avoid errors during initial testing. The actual logic will be added in future tasks.

    Let's create an instance of the `HBnBFacade` class in the `__init__.py` file of the `services/` directory:

    ```python
    from app.services.facade import HBnBFacade

    facade = HBnBFacade()
    ```

    This `facade` instance will be used as a [singleton](https://refactoring.guru/design-patterns/singleton) to make sure that only one instance of the `HBnBFacade` class is created and used throughout the application.

6. **Create the Entry Point**

    In the root directory, create the `run.py` file that will serve as the entry point for running the application:

    ```python
    from app import create_app

    app = create_app()

    if __name__ == '__main__':
        app.run(debug=True)
    ```

7. **Prepare the Configuration**

    In the root directory, create a `config.py` file where you can define environment-specific settings. For now, you can start with a basic configuration:

    ```python
    import os

    class Config:
        SECRET_KEY = os.getenv('SECRET_KEY', 'default_secret_key')
        DEBUG = False

    class DevelopmentConfig(Config):
        DEBUG = True

    config = {
        'development': DevelopmentConfig,
        'default': DevelopmentConfig
    }
    ```

    You'll enhance this file as needed in later stages of the project.

8. **Document the Project Setup**

    In the `README.md` file, write a brief overview of the project setup:
    - Describe the purpose of each directory and file.
    - Include instructions on how to install dependencies and run the application.

9. **Install Required Packages**

    In the `requirements.txt` file, list the Python packages needed for the project:

    ```text
    flask
    flask-restx
    ```

    Install the dependencies using:

    ```bash
    pip install -r requirements.txt
    ```

10. **Test the Initial Setup**

    Run the application to ensure everything is set up correctly:

    ```bash
    python run.py
    ```

    You should see the Flask application running, although no routes are functional yet. This confirms that the project structure and basic setup are correct and ready for further development.

## Expected Outcome

By the end of this task, you'll have a well-organized, modular project structure with clear separation of concerns across the Presentation, Business Logic, and Persistence layers. The Flask application will be functional, with an in-memory repository and Facade pattern in place, ready for future integration of API endpoints and a database-backed persistence layer.

### Resources

- [**Flask Documentation**](https://flask.palletsprojects.com/)
- [**Flask-RESTx Documentation**](https://flask-restx.readthedocs.io/)
- [**Python Project Structure Best Practices**](https://docs.python-guide.org/writing/structure/)
- [**Facade Design Pattern in Python**](https://refactoring.guru/design-patterns/facade/python/example)
