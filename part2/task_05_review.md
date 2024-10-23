# Implement the Review Endpoints

## Context

This task involves setting up the endpoints to handle CRUD operations (Create, Read, Update) for reviews, while ensuring integration with the Business Logic layer via the Facade pattern.

The `DELETE` operation **will be implemented** for reviews, making it the only entity for which deletion is supported in this part of the project.

## In this task, you will

1. Set up the `POST`, `GET`, `PUT`, and `DELETE` endpoints for managing reviews.
2. Implement the logic for handling review-related operations in the Business Logic layer.
3. Integrate the Presentation layer (API) and Business Logic layer through the Facade.
4. Implement validation for specific attributes like the text and rating of the review, and ensure that the review is associated with both a user and a place.
5. Update the Place model in `api/v1/places.py` to consider the collection of reviews for a place.

## Instructions

### Implement the Review Management Logic in the Business Logic Layer

In the `models/review.py` file, the `Review` class should already be implemented from Task 2. Ensure that the class can handle relationships with users and places.

Update the `HBnBFacade` class in the `services/facade.py` file, adding the following methods:

```python
def create_review(self, review_data):
    # Placeholder for logic to create a review, including validation for user_id, place_id, and rating
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

These methods manage review creation, retrieval, updates, and deletion. You need to implement validation to ensure that the `user_id`, `place_id`, and `rating` are valid and correspond to existing users and places.

### Implement the Review Endpoints in the Presentation Layer (API)

Create the `api/v1/reviews.py` file, then define the routes and create the skeleton methods for these endpoints. Use the placeholders provided below to get started.

```python
from flask_restx import Namespace, Resource, fields
from app.services import facade

api = Namespace('reviews', description='Review operations')

# Define the review model for input validation and documentation
review_model = api.model('Review', {
    'text': fields.String(required=True, description='Text of the review'),
    'rating': fields.Integer(required=True, description='Rating of the place (1-5)'),
    'user_id': fields.String(required=True, description='ID of the user'),
    'place_id': fields.String(required=True, description='ID of the place')
})

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

- `POST /api/v1/reviews/`: Register a new review.
- `GET /api/v1/reviews/`: Return a list of all reviews.
- `GET /api/v1/reviews/<review_id>`: Retrieve details of a specific review.
- `GET /api/v1/places/<place_id>/reviews`: Retrieve all reviews for a specific place.
- `PUT /api/v1/reviews/<review_id>`: Update a review’s information.
- `DELETE /api/v1/reviews/<review_id>`: Delete a review.

**Explanation:**

- The `POST` endpoint handles the creation of a new review, while `GET`, `PUT`, and `DELETE` endpoints manage review retrieval, updates, and deletion.
- The `GET /api/v1/places/<place_id>/reviews` endpoint is specific to retrieving all reviews associated with a particular place.
- Placeholders are provided, but you need to implement the logic that handles relationships between reviews, users, and places.

> [!IMPORTANT]
> **Remember to register the namespace and API documentation for the amenity endpoints in the `api/__init__.py` file.**

### Update the Place Model to Include Reviews

In the `api/v1/places.py` file, update the `place_model` to include the collection of reviews for a place. This ensures that when retrieving place details, all associated reviews are included.

**Updated Place Model:**

```python
# Adding the review model
review_model = api.model('PlaceReview', {
    'id': fields.String(description='Review ID'),
    'text': fields.String(description='Text of the review'),
    'rating': fields.Integer(description='Rating of the place (1-5)'),
    'user_id': fields.String(description='ID of the user')
})

place_model = api.model('Place', {
    'title': fields.String(required=True, description='Title of the place'),
    'description': fields.String(description='Description of the place'),
    'price': fields.Float(required=True, description='Price per night'),
    'latitude': fields.Float(required=True, description='Latitude of the place'),
    'longitude': fields.Float(required=True, description='Longitude of the place'),
    'owner_id': fields.String(required=True, description='ID of the owner'),
    'owner': fields.Nested(user_model, description='Owner of the place'),
    'amenities': fields.List(fields.Nested(amenity_model), description='List of amenities'),
    'reviews': fields.List(fields.Nested(review_model), description='List of reviews')
})
   ```

## Input and Output Formats, Status Codes

For each endpoint, ensure that the input format, output format, and status codes are consistent and clearly defined.

### Test the Provided Endpoints

Once the endpoints are implemented, use tools like Postman or cURL to test each operation.
Ensure that your endpoints are working as expected. Here are some examples:

#### Register a New Review (POST /api/v1/reviews/)

```http
POST /api/v1/reviews/
Content-Type: application/json

{
  "text": "Great place to stay!",
  "rating": 5,
  "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

Expected Response:

```jsonc
{
  "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
  "text": "Great place to stay!",
  "rating": 5,
  "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
}

// 201 Created
```

Possible Status Codes:

- 201 Created: When the review is successfully created.
- 400 Bad Request: If input data is invalid.

#### Retrieve All Reviews (GET /api/v1/reviews/)

```http
GET /api/v1/reviews/
```

Expected Response:

```jsonc
[
  {
    "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
    "text": "Great place to stay!",
    "rating": 5
  },
  ...
]

// 200 OK
```

Possible Status Codes:

- 200 OK: List of reviews retrieved successfully.

#### Retrieve a Review’s Details (GET /api/v1/reviews/<review_id>)

```http
GET /api/v1/reviews/<review_id>
```

Expected Response:

```jsonc
{
  "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
  "text": "Great place to stay!",
  "rating": 5,
  "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "place_id": "1fa85f64-5717-4562-b3fc-2c963f66afa6"
}

// 200 OK
```

Possible Status Codes:

- 200 OK: When the review is successfully retrieved.
- 404 Not Found: If the review does not exist.

#### Update a Review’s Information (PUT /api/v1/reviews/<review_id>)

```http
PUT /api/v1/reviews/<review_id>
Content-Type: application/json

{
  "text": "Amazing stay!",
  "rating": 4
}
```

Expected Response:

```jsonc
{
  "message": "Review updated successfully"
}

// 200 OK
```

Possible Status Codes:

- 200 OK: When the review is successfully updated.
- 404 Not Found: If the review does not exist.
- 400 Bad Request: If input data is invalid.

#### Delete a Review (DELETE /api/v1/reviews/<review_id>)

```http
DELETE /api/v1/reviews/<review_id>
```

Expected Response:

```jsonc
{
  "message": "Review deleted successfully"
}

// 200 OK
```

Possible Status Codes:

- 200 OK: When the review is successfully deleted.
- 404 Not Found: If the review does not exist.

#### Retrieve All Reviews for a Specific Place (GET /api/v1/places/<place_id>/reviews)

```http
GET /api/v1/places/<place_id>/reviews
```

Expected Response:

```jsonc
[
  {
    "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
    "text": "Great place to stay!",
    "rating": 5
  },
  {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "text": "Very comfortable and clean.",
    "rating": 4
  }
]

// 200 OK
```

Possible Status Codes:

- 200 OK: List of reviews for the place retrieved successfully.
- 404 Not Found: If the place does not exist.

## Expected Outcome

By the end of this task, you should have implemented the core review management endpoints, including the ability to create, read, update, and delete reviews. Additionally, you will have implemented the ability to retrieve all reviews associated with a specific place. The functionality should be documented and tested, ensuring that all review-related operations are handled smoothly within the HBnB application.

## Resources

- [**Flask-RESTx Documentation**](https://flask-restx.readthedocs.io/)
- [**Testing REST APIs with cURL**](https://everything.curl.dev/)
- [**Designing RESTful APIs**](https://restfulapi.net/)
