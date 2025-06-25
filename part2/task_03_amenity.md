# Implement the Amenity Endpoints

## Context

This task involves setting up the endpoints to handle CRUD operations (Create, Read, Update) for amenities, while ensuring integration with the Business Logic layer via the Facade pattern. The `DELETE` operation will **not** be implemented for amenities in this part of the project.

## In this task, you will

1. Set up the `POST`, `GET`, and `PUT` endpoints for managing amenities.
2. Implement the necessary logic for handling amenity-related operations in the Business Logic layer.
3. Integrate the Presentation layer (API) and Business Logic layer through the Facade.

## Instructions: Detailed Guide to get you started

### Implement the Business Logic Layer

In the `models/amenity.py` file, the `Amenity` class should have already been implemented in Task 2.

Update the `HBnBFacade` class in the `services/facade.py` file, adding the following methods:

```python
def create_amenity(self, amenity_data):
    # Placeholder for logic to create an amenity
    pass

def get_amenity(self, amenity_id):
    # Placeholder for logic to retrieve an amenity by ID
    pass

def get_all_amenities(self):
    # Placeholder for logic to retrieve all amenities
    pass

def update_amenity(self, amenity_id, amenity_data):
    # Placeholder for logic to update an amenity
    pass
```

These methods manage the creation, retrieval, and updating of amenities within the Business Logic layer. You will need to fill in the logic that handles interactions with the repository and implements necessary validation.

### Implement the Amenity Endpoints in the Presentation Layer (API)

Create the `api/v1/amenities.py` file, then define the routes and create the skeleton methods for these endpoints. Use the placeholders provided below to get started.

```python
from flask_restx import Namespace, Resource, fields
from app.services import facade

api = Namespace('amenities', description='Amenity operations')

# Define the amenity model for input validation and documentation
amenity_model = api.model('Amenity', {
    'name': fields.String(required=True, description='Name of the amenity')
})

@api.route('/')
class AmenityList(Resource):
    @api.expect(amenity_model)
    @api.response(201, 'Amenity successfully created')
    @api.response(400, 'Invalid input data')
    def post(self):
        """Register a new amenity"""
        # Placeholder for the logic to register a new amenity
        pass

    @api.response(200, 'List of amenities retrieved successfully')
    def get(self):
        """Retrieve a list of all amenities"""
        # Placeholder for logic to return a list of all amenities
        pass

@api.route('/<amenity_id>')
class AmenityResource(Resource):
    @api.response(200, 'Amenity details retrieved successfully')
    @api.response(404, 'Amenity not found')
    def get(self, amenity_id):
        """Get amenity details by ID"""
        # Placeholder for the logic to retrieve an amenity by ID
        pass

    @api.expect(amenity_model)
    @api.response(200, 'Amenity updated successfully')
    @api.response(404, 'Amenity not found')
    @api.response(400, 'Invalid input data')
    def put(self, amenity_id):
        """Update an amenity's information"""
        # Placeholder for the logic to update an amenity by ID
        pass
```

- `POST /api/v1/amenities/`: Register a new amenity.
- `GET /api/v1/amenities/`: Retrieve a list of all amenities.
- `GET /api/v1/amenities/<amenity_id>`: Get amenity details by ID.
- `PUT /api/v1/amenities/<amenity_id>`: Update an amenity's information.

**Explanation:**

- The `POST` endpoint handles the creation of a new amenity, while the `GET` endpoints manage retrieval, both for a single amenity and a list of all amenities. The `PUT` endpoint is responsible for updating an existing amenity’s details.
- The placeholders give you a foundation to build on, while you will need to implement the logic based on previous examples like the user registration task.

> [!IMPORTANT]
> **Remember to register the namespace and API documentation for the amenity endpoints in the `app/__init__.py` file.**

## Input and Output Formats, Status Codes

For each endpoint, ensure that the input format, output format, and status codes are consistent and clearly defined.

### Test the Provided Endpoints

Once the endpoints are implemented, use tools like Postman or cURL to test each operation.
Ensure that your endpoints are working as expected. Here are some examples:

#### Register a New Amenity (POST /api/v1/amenities/)

```http
POST /api/v1/amenities/
Content-Type: application/json

{
  "name": "Wi-Fi"
}
```

Expected Response:

```jsonc
{
  "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "Wi-Fi"
}

// 201 Created
```

Possible Status Codes:

- 201 Created: When the amenity is successfully created.
- 400 Bad Request: If input data is invalid.

#### Retrieve All Amenities (GET /api/v1/amenities/)

```http
GET /api/v1/amenities/
Content-Type: application/json
```

Expected Response:

```jsonc
[
  {
    "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "Wi-Fi"
  },
  {
    "id": "2fa85f64-5717-4562-b3fc-2c963f66afa6",
    "name": "Air Conditioning"
  }
]

// 200 OK
```

Possible Status Codes:

- 200 OK: List of amenities retrieved successfully.

#### Retrieve an Amenity’s Details (GET /api/v1/amenities/<amenity_id>)

```http
GET /api/v1/amenities/<amenity_id>
Content-Type: application/json
```

Expected Response:

```jsonc
{
  "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "Wi-Fi"
}

// 200 OK
```

Possible Status Codes:

- 200 OK: When the amenity is successfully retrieved.
- 404 Not Found: If the amenity does not exist.

#### Update an Amenity’s Information (PUT /api/v1/amenities/<amenity_id>)

```http
PUT /api/v1/amenities/<amenity_id>
Content-Type: application/json

{
  "name": "Air Conditioning"
}
```

Expected Response:

```jsonc
{
  "message": "Amenity updated successfully"
}

// 200 OK
```

Possible Status Codes:

- 200 OK: When the amenity is successfully updated.
- 404 Not Found: If the amenity does not exist.
- 400 Bad Request: If input data is invalid.

## Expected Outcome

By the end of this task, you should have fully implemented the core amenity management endpoints, including the ability to create, read, and update amenities. The functionality should be documented and tested, ensuring that all amenity-related operations are handled smoothly within the HBnB application.

## Resources

- [**Flask-RESTx Documentation**](https://flask-restx.readthedocs.io/)
- [**Testing REST APIs with cURL**](https://everything.curl.dev/)
- [**Designing RESTful APIs**](https://restfulapi.net/)
