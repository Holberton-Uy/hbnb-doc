### Task: Modify the User Model to Include Password Hashing

#### Objective
Update the `User` model to securely store a hashed password using **bcrypt**. Modify the user registration endpoint to accept the `password` field and ensure that it is hashed before storing it. The password should **not** be returned in any `GET` requests.

#### Context
In previous tasks, the `User` model was created, but it did not handle passwords. In this task, you will enhance the `User` model to support password hashing using **bcrypt**, ensuring passwords are securely stored. Additionally, you will update the user registration endpoint to accept and hash passwords before storing them. Passwords will not be returned in any `GET` requests.

#### Instructions

1. **Install the `flask-bcrypt` plugin**
   - In your `requirements.txt` file, add the following line:
     ```
     flask-bcrypt
     ```
   - Run the following command to install it:
     ```bash
     pip install flask-bcrypt
     ```

2. **Register the plugin within the Application**
   - In the `app/__init__.py` file:
     - Import `Bcrypt` from `flask_bcrypt` package and instantiate it.
       
       ```python
       from flask_bcrypt import Bcrypt
      
       bcrypt = Bcrypt()
       ```
 
     - Initialize the instance in the `create_app()` function.
       ```python
       def create_app(config_class=config.DevelopmentConfig):
          #
          # Existent code with app Flask instance
          # ...
          bcrypt.init_app(app)
       ```

2. **Update the `User` Model to Include Password Hashing**
   - In the `models/user.py` file:
      - update the `User` class to include a `password` attribute. This field will store the hashed version of the user's password.

   - Implement the following methods in the `User` class:
     - **hash_password(password):** This function should take a plaintext password, hash it using **bcrypt**, and store the hashed version in the `password` field.
       
       ```python
       def hash_password(self, password):
           """Hashes the password before storing it."""
           self.password = bcrypt.generate_password_hash(password).decode('utf-8')
       ```
       
     - **verify_password(password):** This function should compare a plaintext password with the hashed password stored in the `password` field, returning `True` if they match and `False` otherwise.
       
       ```python
       def verify_password(self, password):
           """Verifies if the provided password matches the hashed password."""
           return bcrypt.check_password_hash(self.password, password)
       ```

3. **Modify the User Registration Endpoint (`POST /api/v1/users/`)**
   - Modify the user registration endpoint to accept a `password` field in addition to the first name, last name, and email.
   - Ensure that the `password` is hashed using the `hash_password` function before storing the user in the repository.
   - After registering the user, do **not** return the password in the response. You will only return the user's ID and a success message.

4. **Modify the User Retrieval Endpoint (`GET /api/v1/users/<user_id>`)**
   - Update the `GET` endpoint to **exclude** the `password` from the response when retrieving a user's details.

5. **Test the Password Hashing Logic**
   - Use Postman or cURL to test the POST /api/v1/users/ endpoint, ensuring that the password is hashed and not returned in the response.
   - Verify that the GET /api/v1/users/<user_id> endpoint does not expose the password field.

#### Resources
- **Flask-Bcrypt Documentation:** [Flask-Bcrypt](https://flask-bcrypt.readthedocs.io/en/latest/)
- **Password Hashing Best Practices:** [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)

#### Expected Outcome
By the end of this task, the `User` model will securely hash and store passwords using **bcrypt**. The `POST /api/v1/users/` endpoint will accept passwords and securely hash them before storing. Passwords will not be returned in any `GET` requests.
