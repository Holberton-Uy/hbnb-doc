### Task: Implement JWT Authentication with `flask-jwt-extended`

#### Objective
Set up JWT-based authentication for the HBnB application, enabling secure login functionality. This task involves configuring the API to generate and verify JWT tokens using the `flask-jwt-extended` extension. Tokens will be issued upon successful login and required for accessing protected endpoints.

#### Context
JWT (JSON Web Token) allows secure authentication by providing a token that clients can use to access protected resources without having to re-authenticate on every request. JWT is stateless, meaning the server doesn’t need to store user sessions, making it ideal for scalable applications. JWT tokens also allow embedding additional claims (such as user roles), which is useful for authorization. 

In this task, we will set up user login, issue JWT tokens, and use these tokens to protect specific API endpoints.

Here is a **Mermaid.js diagram** that shows the interaction between the client and the API for obtaining a JWT token and using it to make authenticated requests:

##### Adding a Middleware

In Flask, **middleware** is software that sits between the request and the final handling of that request by your Flask routes. Middleware can manipulate requests, responses, or both. 

In the context of JWT authentication, middleware is responsible for verifying the JWT token with each incoming request. Flask-JWT-Extended provides decorators like `@jwt_required()` that act as middleware, ensuring that a valid token is present before allowing access to protected resources.

- **What does middleware do in JWT?**:
Middleware intercepts the request and checks if the token is present and valid. If the token is invalid or missing, the middleware prevents the request from reaching the route handler and instead returns an error response.

- **How does Flask-JWT-Extended handle this?**:
Flask-JWT-Extended makes use of decorators such as `@jwt_required()` and `@jwt_optional()`. These decorators ensure that protected routes are only accessible if the user has a valid token.

```mermaid
sequenceDiagram
    participant Client
    participant API
    participant JWTManager

    Client->>+API: POST /api/v1/auth/login (email, password)
    API->>+JWTManager: Validate credentials and generate JWT
    JWTManager-->>-API: Returns JWT token
    API-->>-Client: Returns JWT token in response

    Client->>+API: GET /api/v1/protected (Authorization: Bearer JWT)
    API->>+JWTManager: Validate JWT token
    JWTManager-->>-API: Valid token, return user info
    API-->>-Client: Protected data response
```

This sequence diagram visualizes the flow where:

1. **Client Login Request**: 
   - The client initiates the login process by sending a `POST` request to the API endpoint (`/api/v1/auth/login`). In the request, the client provides the user's credentials (email and password).
   
2. **API and JWT Token Generation**:
   - The API checks the provided credentials, and if they are valid, the API uses the **JWTManager** to generate a JWT (JSON Web Token). This token is a secure representation of the user’s identity, containing claims like `is_admin` and an expiration time.

3. **JWT Token Response**:
   - The API responds to the client with the generated JWT token. The token is sent in the body of the response and is used by the client to authenticate further API requests.

4. **Client Makes Authenticated Requests**:
   - After obtaining the JWT token, the client sends another request to a protected endpoint (e.g., `GET /api/v1/protected`). This time, the request includes the token in the `Authorization` header as `Bearer JWT_TOKEN`. 

5. **JWT Token Validation**:
   - The API uses the **JWTManager** to validate the token. If the token is valid (not expired, tampered with, or revoked), the API proceeds to process the request.

6. **Protected Resource Access**:
   - Upon successful validation, the API returns the protected data. If the token is invalid or expired, the API responds with an error message, often a `401 Unauthorized` response.

---

#### Instructions

1. **Install the `flask-jwt-extended` Library**
   - In your `requirements.txt` file, add the following line:
     ```
     flask-jwt-extended
     ```
   - Run the following command to install it:
     ```bash
     pip install flask-jwt-extended
     ```

2. **Configure JWT in the Flask Application**
   - In the `app/__init__.py` file, import and instantiate the `JWTManager` class provided by `flask-jwt-extended` to enable JWT handling in your Flask application. 
   
     ```python
     from flask_jwt_extended import JWTManager

     jwt = JWTManager()
     ```

   - In the `create_app()` function, register the middleware with the application instance.

     ```python
     def create_app(config_class=config.DevelopmentConfig):
          #
          # Existent code with app Flask instance
          # ...
         jwt.init_app(app)
         
     ```
     
     > [!IMPORTANT]
     > The `flask-jwt-extended` uses a key stored in [`JWT_SECRET_KEY`](https://flask-jwt-extended.readthedocs.io/en/stable/options.html#JWT_SECRET_KEY) for securely signing the session cookie. 
     > In this project we're going to use Flask's [SECRET_KEY](https://flask.palletsprojects.com/en/stable/config/#SECRET_KEY) that we've already set in the Configuration Class and can also be used by the plugin.

3. **Create a Login Endpoint (`POST /api/v1/login`)**
   - The login endpoint will authenticate the user, generate a JWT token, and return it to the client. The token will include the user's `id` and `is_admin` claim to differentiate between regular users and admins.

   - In `api/v1/auth.py`, create the following endpoint:
     ```python
     from flask_restx import Namespace, Resource, fields
     from flask_jwt_extended import create_access_token
     from app.services import facade

     api = Namespace('auth', description='Authentication operations')

     # Model for input validation
     login_model = api.model('Login', {
         'email': fields.String(required=True, description='User email'),
         'password': fields.String(required=True, description='User password')
     })

     @api.route('/login')
     class Login(Resource):
         @api.expect(login_model)
         def post(self):
             """Authenticate user and return a JWT token"""
             credentials = api.payload  # Get the email and password from the request payload
             
             # Step 1: Retrieve the user based on the provided email
             user = facade.get_user_by_email(credentials['email'])
             
             # Step 2: Check if the user exists and the password is correct
             if not user or not user.verify_password(credentials['password']):
                 return {'error': 'Invalid credentials'}, 401

             # Step 3: Create a JWT token with the user's id and is_admin flag
             access_token = create_access_token(identity={'id': str(user.id), 'is_admin': user.is_admin})
             
             # Step 4: Return the JWT token to the client
             return {'access_token': access_token}, 200
     ```

   **Explanation:**
   - `create_access_token`: This function generates a JWT token. The token includes an **identity**, which in this case is a dictionary containing the `user.id` and `is_admin` flag. This allows the token to carry claims that we can later use for authorization.
   - **Token Claims**: By adding claims like `id` and `is_admin` to the token, we can embed information about the user's identity and permissions within the token itself. This way, we don't need to query the database for every request to check if a user is an admin.
   
   **Steps Breakdown:**
   - **Step 1**: Retrieve the user by email using the `get_user_by_email` method from the `HBnBFacade`. This method will query the repository (in-memory or database, depending on the part of the project) for a user with the provided email.
   - **Step 2**: Use the `verify_password` method (which should have been implemented in the `User` model) to check if the provided password matches the hashed password stored in the repository.
   - **Step 3**: If the credentials are valid, generate a JWT token with the `create_access_token` function. We embed the `user.id` and `is_admin` flag as part of the token claims, allowing us to identify the user and their role in subsequent requests.
   - **Step 4**: Return the generated token to the client as part of the response body. This token can be used by the client to authenticate future requests.

4. **Protect Endpoints with JWT**
   - Once the JWT is generated, you can protect sensitive API endpoints by requiring that the user include the token in the `Authorization` header of their request. Use the `@jwt_required()` decorator to enforce this.

   Example:
   ```python
   from flask_jwt_extended import jwt_required, get_jwt_identity

   @api.route('/protected')
   class ProtectedResource(Resource):
       @jwt_required()
       def get(self):
           """A protected endpoint that requires a valid JWT token"""
           current_user = get_jwt_identity()  # Retrieve the user's identity from the token
           return {'message': f'Hello, user {current_user["id"]}'}, 200
   ```

   **Explanation:**
   - **@jwt_required()**: This decorator ensures that the endpoint can only be accessed if a valid JWT is provided in the request. If the token is missing or invalid, the request will be rejected.
   - **get_jwt_identity()**: This function extracts the identity (in this case, the `user.id` and `is_admin` claims) from the token. We can then use this information to personalize responses or enforce role-based access.

5. **Test the Authentication Flow**
   Use `curl` to test the login and authentication functionality:
   
   **Login and Get JWT:**
   ```bash
   curl -X POST "http://127.0.0.1:5000/api/v1/auth/login" -H "Content-Type: application/json" -d '{
     "email": "john.doe@example.com",
     "password": "your_password"
   }'
   ```
   **Expected Response:**
   ```json
   {
       "access_token": "your_generated_jwt_token"
   }
   ```

   **Access a Protected Endpoint:**
   ```bash
   curl -X GET "http://127.0.0.1:5000/api/v1/protected" -H "Authorization: Bearer your_generated_jwt_token"
   ```
   **Expected Response:**
   ```json
   {
       "message": "Hello, user 3fa85f64-5717-4562-b3fc-2c963f66afa6"
   }
   ```

#### Resources
- **JWT Authentication**: [JWT.io](https://jwt.io/)
- **Flask-JWT-Extended Documentation:** [Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/)
- **JWT Best Practices:** [OWASP JWT Security](https://cheatsheetseries.owasp.org/cheatsheets/JSON_Web_Token_Cheat_Sheet_for_Java.html)

#### Expected Outcome
By the end of this task, students will have implemented user login functionality using JWT. They will be able to generate and verify JWT tokens, protect specific API endpoints, and embed claims (like `is_admin`) within the token for future authorization checks.
