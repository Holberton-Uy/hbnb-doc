### Task: Implement Authenticated User Access Endpoints

#### Objective
Secure various API endpoints to allow only authenticated users to perform specific actions, including creating and modifying places and reviews, as well as updating their own user details. Access will be controlled via JWT authentication, with additional validation to ensure users can only modify data that belongs to them (e.g., places they own, reviews they created).

#### Context
Authenticated user access is a critical part of securing an API. By ensuring that only authorized users can perform specific actions, the integrity of the data is protected. This task focuses on securing endpoints related to creating and modifying places and reviews, while also allowing users to modify their own data.

In this task, you will:
1. Secure endpoints to ensure only authenticated users can create, update, and delete resources.
2. Add logic to validate ownership of places and reviews.
3. Implement logic to prevent users from reviewing places they own or reviewing a place multiple times.
4. Verify that public users can access the **PUBLIC** endpoints without a JWT token.

---

#### Instructions

1. **Secure the Endpoints with JWT Authentication**
   The following endpoints will be protected by JWT authentication, and additional validation will be added to ensure that users can only modify their own data:

   - **POST /api/v1/places/**: Create a new place.
   - **PUT /api/v1/places/<place_id>**: Update a place's details. Only the owner of the place can modify its information.
   - **POST /api/v1/reviews/**: Create a new review. Users can only review places they do **not** own and can only create **one** review per place.
   - **PUT /api/v1/reviews/<review_id>**: Update a review. Users can only modify reviews they created.
   - **DELETE /api/v1/reviews/<review_id>**: Delete a review. Users can only delete reviews they created.
   - **PUT /api/v1/users/<user_id>**: Modify user information. Users can only modify their own details (excluding email and password).

2. **Identify Public Endpoints**
   - The following endpoints will be publicly accessible:
     - **GET** `/api/v1/places/`: Retrieve a list of available places.
     - **GET** `/api/v1/places/<place_id>`: Retrieve detailed information about a specific place.

3. **Implement the Logic for JWT Authentication**
   Use the `@jwt_required()` decorator from `flask-jwt-extended` to enforce JWT authentication on the above endpoints. The `get_jwt_identity()` function will be used to identify the currently logged-in user.

   **Example:**
   ```python
   from flask_restx import Namespace, Resource, fields
   from flask_jwt_extended import jwt_required, get_jwt_identity

   api = Namespace('places', description='Place operations')

   @api.route('/')
   class PlaceList(Resource):
       @jwt_required()
       def post(self):
           """Create a new place"""
           current_user = get_jwt_identity()
           # Logic to create a new place for the logged-in user
           pass
   ```

4. **Validation Logic**

   - **Create Place (POST /api/v1/places/):**
     - Ensure the user is authenticated.
     - The `owner_id` of the place must be set to the ID of the authenticated user (retrieved using `get_jwt_identity()`).

    **Example:**
    ```python
    @api.route('/<place_id>')
    class PlaceResource(Resource):
        @jwt_required()
        def put(self, place_id):
            current_user = get_jwt_identity()
            place = facade.get_place(place_id)
            if place.owner_id != current_user:
                return {'error': 'Unauthorized action'}, 403
            # Logic to update the place
            pass
    ```
   
   - **Update Place (PUT /api/v1/places/<place_id>):**
     - Ensure the user is authenticated.
     - Check that the `owner_id` of the place matches the ID of the authenticated user.
     - If the user is not the owner, return a 403 error with the message "Unauthorized action."

   - **Create Review (POST /api/v1/reviews/):**
     - Ensure the user is authenticated.
     - Check that the `place_id` in the request belongs to a place the user does **not** own.
     - Check that the user has not already reviewed this place.
     - If the user is the owner of the place, return a 400 error with the message "You cannot review your own place."
     - If the user has already reviewed the place, return a 400 error with the message "You have already reviewed this place."

   - **Update Review (PUT /api/v1/reviews/<review_id>):**
     - Ensure the user is authenticated.
     - Check that the `user_id` of the review matches the authenticated user.
     - If the user did not create the review, return a 403 error with the message "Unauthorized action."

   - **Delete Review (DELETE /api/v1/reviews/<review_id>):**
     - Ensure the user is authenticated.
     - Check that the `user_id` of the review matches the authenticated user.
     - If the user did not create the review, return a 403 error with the message "Unauthorized action."

   - **Modify User Information (PUT /api/v1/users/<user_id>):**
     - Ensure the user is authenticated.
     - Check that the `user_id` in the URL matches the authenticated user.
     - Prevent the user from modifying their `email` and `password` in this endpoint.
     - If the user tries to modify their email or password, return a 400 error with the message "You cannot modify email or password."
     - If the user is trying to modify another user's data, return a 403 error with the message "Unauthorized action."

---

5. **Test the Authenticated Endpoints**

   Use Postman or cURL to test these authenticated endpoints. Ensure that unauthorized actions (e.g., modifying a place that the user does not own) return the appropriate error messages.

   **Test Place Creation (POST /api/v1/places/):**
   ```bash
   curl -X POST "http://127.0.0.1:5000/api/v1/places/" -d '{"title": "New Place"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Unauthorized Place Update (PUT /api/v1/places/<place_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/places/<place_id>" -d '{"title": "Updated Place"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Expected Response for Unauthorized Action:**
   ```json
   {
       "error": "Unauthorized action"
   }
   ```

   **Test Creating a Review (POST /api/v1/reviews/):**
   ```bash
   curl -X POST "http://127.0.0.1:5000/api/v1/reviews/" -d '{"place_id": "<place_id>", "text": "Great place!"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Updating a Review (PUT /api/v1/reviews/<review_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/reviews/<review_id>" -d '{"text": "Updated review"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

   **Test Deleting a Review (DELETE /api/v1/reviews/<review_id>):**
   ```bash
   curl -X DELETE "http://127.0.0.1:5000/api/v1/reviews/<review_id>" -H "Authorization: Bearer <your_token>"
   ```

   **Test Modifying User Data (PUT /api/v1/users/<user_id>):**
   ```bash
   curl -X PUT "http://127.0.0.1:5000/api/v1/users/<user_id>" -d '{"first_name": "Updated Name"}' -H "Authorization: Bearer <your_token>" -H "Content-Type: application/json"
   ```

6. **Test the Public Endpoints**
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
2. **Testing REST APIs with cURL:** [Everything cURL](https://everything.curl.dev/)

---

#### Expected Outcome

By the end of this task, the following endpoints will be secured, allowing only authenticated users to perform actions based on their ownership of places and reviews:
- Create, update, and delete places (with ownership checks).
- Create and update reviews (with restrictions on reviewing owned places and duplicate reviews).
- Modify user details (excluding email and password).
