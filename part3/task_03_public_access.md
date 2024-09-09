### Task 3: Implement Public Access Endpoints

#### Objective
Configure specific API endpoints to be publicly accessible without requiring user authentication. These endpoints will allow public users to view a list of available places and access detailed information for each place, without the need to log in.

#### Context
In any application, some features should be accessible to users without requiring them to authenticate. For HBnB, this means allowing public users to browse available places and view detailed information about each place without having to log in or provide a JWT token. This task focuses on identifying and configuring those endpoints, ensuring that public users can access them.

In this task, you will:
1. Identify the endpoints for publicly accessible data.
2. Configure these endpoints to bypass authentication requirements using `flask-jwt-extended`.
3. Test that public users can access these endpoints without a JWT token.

---

#### Instructions

1. **Identify Public Endpoints**
   - For this task, the following endpoints will be publicly accessible:
     - **GET** `/api/v1/places/`: Retrieve a list of available places.
     - **GET** `/api/v1/places/<place_id>`: Retrieve detailed information about a specific place.

2. **Configure Endpoints for Public Access**
   - Using `flask-jwt-extended`, these endpoints should **not** include the `@jwt_required()` decorator, allowing public access without requiring a token.

3. **Update the Namespace Registration**
   - Ensure that the namespace for places is registered in `app/__init__.py`, so these endpoints are properly exposed:

4. **Test the Public Endpoints**
   - Test the endpoints to verify they can be accessed without a JWT token.

   **Example Test Using cURL:**
   - Retrieve a list of places:
     ```bash
     curl -X GET "http://127.0.0.1:5000/api/v1/places/"
     ```

   **Expected Response:**
   ```json
   [
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "title": "Cozy Apartment",
           "price": 100.0
       },
       {
           "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
           "title": "Luxury Condo",
           "price": 200.0
       }
   ]
   ```

   - Retrieve detailed information about a specific place:
     ```bash
     curl -X GET "http://127.0.0.1:5000/api/v1/places/1fa85f64-5717-4562-b3fc-2c963f66afa6"
     ```

   **Expected Response:**
   ```json
   {
       "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
       "title": "Cozy Apartment",
       "description": "A comfortable and affordable place to stay.",
       "price": 100.0,
       "latitude": 37.7749,
       "longitude": -122.4194
   }
   ```

---

#### Resources

1. **Flask-JWT-Extended Documentation:** [Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/)
2. **REST API Best Practices for Public Endpoints:** [REST API Security](https://restfulapi.net/security-essentials/)
3. **Testing REST APIs with cURL:** [Everything cURL](https://everything.curl.dev/)

---

#### Expected Outcome

By the end of this task, public users should be able to access the list of available places and detailed information about individual places without needing to authenticate. You will test these endpoints to ensure they can be accessed without a JWT token.
