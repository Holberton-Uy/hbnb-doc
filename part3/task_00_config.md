### Task: Modify the Application Factory to Include the Configuration

#### Objective
Update the Flask Application Factory to include the configuration object.

#### Context
In the previous part of the project, we created a `Config` class to handle different configurations in the application, but we weren't using it yet. In this task, you will update the `create_app()` method (following the Application Factory pattern) in the `app/__init__.py` file to receive a configuration, which will be used to instantiate the application.

> [!IMPORTANT]
> Before starting the task, be sure to read and understand the resources provided below.

#### Instructions

1. **Update the `create_app()` method to receive a configuration**
   - In the `app/__init__.py` file:
     - Update the `create_app()` method to receive a configuration object.
     - Modify the method to instantiate the Flask application using the configuration object.
     - Return the created application instance.

   ```python
   def create_app(config_class="config.DevelopmentConfig"):
       app = Flask(__name__)
       app.config.from_object(config_class)
       # ...
   ```

   We'll set the `config.DevelopmentConfig` class as the default.

#### Resources
- **Flask Documentation:** [Application Factories](https://flask.palletsprojects.com/en/stable/patterns/appfactories/)
- **Flask Documentation:** [Configuration Handling](https://flask.palletsprojects.com/en/stable/config/)

#### Expected Outcome
By the end of this task, you should have a fully functional Application Factory that can handle different configurations.
