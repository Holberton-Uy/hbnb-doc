### Task 6: Implement the Review Endpoints

#### Objective
Implement the API endpoints needed for managing reviews in the HBnB application. This task involves setting up CRUD operations (Create, Read, Update) for reviews, ensuring that these endpoints are integrated with the Business Logic layer through the Facade pattern. The `DELETE` operation **will** be implemented for reviews, making it the only entity for which deletion is supported in this part of the project.

In this task, you will:
1. Set up the `POST`, `GET`, `PUT`, and `DELETE` endpoints for managing reviews.
2. Implement the logic for handling review-related operations in the Business Logic layer.
3. Integrate the Presentation layer (API) and Business Logic layer through the Facade.
4. Implement validation for specific attributes like the text of the review and ensure that the review is associated with both a user and a place.

#### Instructions

1. **Set Up the Review Endpoints in the Presentation Layer (API)**
   - In the `api/v1/reviews.py` file, define the following endpoints:
     - `POST /api/v1/reviews/`: Register a new review.
     - `GET /api/v1/reviews/`: Return a list of all reviews.
     - `GET /api/v1/reviews/<review_id>`: Retrieve details of a specific review.
     - `GET /api/v1/places/<place_id>/reviews`: Retrieve all reviews for a specific place.
     - `PUT /api/v1/reviews/<review_id>`: Update a review’s information.
     - `DELETE /api/v1/reviews/<review_id>`: Delete a review.

   Provide placeholders for the code, allowing students to implement the logic based on the principles already covered in the previous tasks.

   **Placeholders:**
   ```python
   from flask_restx import Namespace, Resource, fields
   from app.services.facade import HBnBFacade

   api = Namespace('reviews', description='Review operations')

   # Define the review model for input validation and documentation
   review_model = api.model('Review', {
       'text': fields.String(required=True, description='Text of the review'),
       'user_id': fields.String(required=True, description='ID of the user'),
       'place_id': fields.String(required=True, description='ID of the place')
   })

   facade = HBnBFacade()

   @api.route('/')
   class ReviewList(Resource):
       @api.expect(review_model)
       @api.response(201, 'Review successfully created')
       @api.response(400, 'Invalid input data')
       def post(self):
           """Register a new review"""
           # Placeholder for the logic to register a new review
           pass

       @api.response(200, 'List of reviews retrieved successfully')
       def get(self):
           """Retrieve a list of all reviews"""
           # Placeholder for logic to return a list of all reviews
           pass   

   @api.route('/<review_id>')
   class ReviewResource(Resource):
       @api.response(200, 'Review details retrieved successfully')
       @api.response(404, 'Review not found')
       def get(self, review_id):
           """Get review details by ID"""
           # Placeholder for the logic to retrieve a review by ID
           pass

       @api.expect(review_model)
       @api.response(200, 'Review updated successfully')
       @api.response(404, 'Review not found')
       @api.response(400, 'Invalid input data')
       def put(self, review_id):
           """Update a review's information"""
           # Placeholder for the logic to update a review by ID
           pass

       @api.response(200, 'Review deleted successfully')
       @api.response(404, 'Review not found')
       def delete(self, review_id):
           """Delete a review"""
           # Placeholder for the logic to delete a review
           pass

   @api.route('/places/<place_id>/reviews')
   class PlaceReviewList(Resource):
       @api.response(200, 'List of reviews for the place retrieved successfully')
       @api.response(404, 'Place not found')
       def get(self, place_id):
           """Get all reviews for a specific place"""
           # Placeholder for logic to return a list of reviews for a place
           pass
   ```

   **Explanation:**
   - The `POST` endpoint handles the creation of a new review, while `GET`, `PUT`, and `DELETE` endpoints manage review retrieval, updates, and deletion.
   - The `GET /api/v1/places/<place_id>/reviews` endpoint is specific to retrieving all reviews associated with a particular place.
   - Placeholders are provided, but you need to implement the logic that handles relationships between reviews, users, and places.

2. **Implement the Review Management Logic in the Business Logic Layer**
   - In the `models/review.py` file, the `Review` class should already be implemented from Task 2. Ensure that the class can handle relationships with users and places.

   **Placeholders for Facade Methods:**
   ```python
   def create_review(self, review_data):
       # Placeholder for logic to create a review, including validation for user_id and place_id
       pass

   def get_review(self, review_id):
       # Placeholder for logic to retrieve a review by ID
       pass

   def get_all_reviews(self):
       # Placeholder for logic to retrieve all reviews
       pass

   def get_reviews_by_place(self, place_id):
       # Placeholder for logic to retrieve all reviews for a specific place
       pass

   def update_review(self, review_id, review_data):
       # Placeholder for logic to update a review
       pass

   def delete_review(self, review_id):
       # Placeholder for logic to delete a review
       pass
   ```

   **Explanation:**
   - The `create_review`, `get_review`, `get_all_reviews`, `get_reviews_by_place`, `update_review`, and `delete_review` methods manage review creation, retrieval, updates, and deletion.
   - You need to implement validation to ensure that the `user_id` and `place_id` are valid and correspond to existing users and places.

3. **Input and Output Formats, Status Codes**

   For each endpoint, ensure that the input format, output format, and status codes are consistent and clearly defined:

   - **POST /api/v1/reviews/** (Register a new review)
     - **Input:**
       ```json
       {
           "text": "Great place to stay!",
           "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
           "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
       }
       ```
     - **Output:**
       ```json
       {
           "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
           "message": "Review created successfully"
       }
       ```
     - **Status Codes:**
       - `201 Created`: When the review is successfully created.
       - `400 Bad Request`: If input data is invalid.

   - **GET /api/v1/reviews/** (Retrieve all reviews)
     - **Output:**
       ```json
       [
          {
              "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
              "text": "Great place to stay!"
          },
          ...
       ]
       ```
     - **Status Codes:**
       - `200 OK`: List of reviews retrieved successfully.

   - **GET /api/v1/reviews/<review_id>** (Retrieve a review’s details)
     - **Output:**
       ```json
       {
           "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
           "text": "Great place to stay!",
           "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
           "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the review is successfully retrieved.
       - `404 Not Found`: If the review does not exist.

   - **PUT /api/v1/reviews/<review_id>** (Update a review’s information)
     - **Input:**
       ```json
       {
           "text": "Amazing stay!"
       }
       ```
     - **Output:**
       ```json
       {
           "message": "Review updated successfully"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the review is successfully updated.
       - `404 Not Found`: If the review does not exist.
       - `400 Bad Request`: If input data is invalid.

   - **DELETE /api/v1/reviews/<review_id>** (Delete a review)
     - **Output:**
       ```json
       {
           "message": "Review deleted successfully"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the review is successfully deleted.
       - `404 Not Found`: If the review does not exist.

   - **GET /api/v1/places/<place_id>/reviews** (Retrieve all reviews for a specific place)
     - **Output:**
       ```json
       [
          {
              "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
              "text": "Great place to stay!"
          },
          {
              "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
              "text": "Very comfortable and clean."
          }
       ]
       ```
     - **Status Codes:**
       - `200 OK`: List of reviews for the place retrieved successfully.
       - `404 Not Found`: If the place does not exist.

5. **Testing the Endpoints**
   - Once the endpoints are implemented, use tools like Postman or cURL to test each operation:
     - **POST**: Register a new review.
     - **GET**: Retrieve a review’s details using its ID or get all reviews for a specific place.
     - **PUT**: Update a review’s information.
     - **DELETE**: Delete a review.

   **Example Test Using cURL:**
   ```bash
   curl -X GET "http://127.0.0.1:5000/api/v1/reviews/<review_id>"
   ```

   **Expected Response:**
   ```json
   {
       "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
       "text": "Great place to stay!",
       "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
       "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
   }
   ```

6. **Document the Review Endpoints**
   - Update the documentation to describe each endpoint:
     - Path, HTTP method, expected payload, and response.
     - How to handle errors (e.g., review not found).

#### Resources

1. **Flask-RESTx Documentation:** [https://flask-restx.readthedocs.io/](https://flask-restx.readthedocs.io/)
2. **Testing REST APIs with cURL:** [https://everything.curl.dev/](https://everything.curl.dev/)
3. **Designing RESTful APIs:** [https://restfulapi.net/](https://restfulapi.net/)

#### Expected Outcome

By the end of this task, you should have implemented the core review management endpoints, including the ability to create, read, update, and delete reviews. Additionally, you will have implemented the ability to retrieve all reviews associated with a specific place. The `DELETE` operation is introduced here to allow students to practice implementing this functionality for the first time. The functionality should be documented and tested, ensuring that all review-related operations are handled smoothly within the HBnB application.
