### Task 5: Implement the Place Endpoints

#### Objective
Implement the API endpoints needed for managing places in the HBnB application. This task involves setting up CRUD operations (Create, Read, Update) for places, ensuring that these endpoints are integrated with the Business Logic layer through the Facade pattern. The `DELETE` operation will **not** be implemented for places in this part of the project.

Given that the `Place` entity has relationships with other entities, such as `User` (owner) and `Amenity`, you’ll need to handle these relationships carefully while maintaining the integrity of the application logic. The `Review` entity will be implemented in the next task, so it should not be included in this task.

In this task, you will:
1. Set up the `POST`, `GET`, and `PUT` endpoints for managing places.
2. Implement the logic for handling place-related operations in the Business Logic layer.
3. Integrate the Presentation layer (API) and Business Logic layer through the Facade.
4. Implement validation for specific attributes like `price`, `latitude`, and `longitude`.
5. Ensure that related data such as `owner` details and `amenities` are properly handled and returned with the Place data.

#### Instructions

1. **Set Up the Place Endpoints in the Presentation Layer (API)**
   - In the `api/v1/places.py` file, define the following endpoints:
     - `POST /api/v1/places/`: Register a new place.
     - `GET /api/v1/places/`: Return a list of all places.
     - `GET /api/v1/places/<place_id>`: Retrieve details of a specific place, including its associated owner and amenities.
     - `PUT /api/v1/places/<place_id>`: Update place information.

   **Define the Place Model:**
   Update the `place_model` to include nested objects for the `owner` and `amenities`. This ensures that when retrieving or updating a place, all related data is properly managed and returned.

   ```python
   from flask_restx import Namespace, Resource, fields
   from app.services.facade import HBnBFacade

   api = Namespace('places', description='Place operations')

   # Define the models for related entities
   amenity_model = api.model('Amenity', {
       'id': fields.String(description='Amenity ID'),
       'name': fields.String(description='Name of the amenity')
   })

   user_model = api.model('User', {
       'id': fields.String(description='User ID'),
       'first_name': fields.String(description='First name of the owner'),
       'last_name': fields.String(description='Last name of the owner'),
       'email': fields.String(description='Email of the owner')
   })

   # Define the place model for input validation and documentation
   place_model = api.model('Place', {
       'title': fields.String(required=True, description='Title of the place'),
       'description': fields.String(description='Description of the place'),
       'price': fields.Float(required=True, description='Price per night'),
       'latitude': fields.Float(required=True, description='Latitude of the place'),
       'longitude': fields.Float(required=True, description='Longitude of the place'),
       'owner_id': fields.String(required=True, description='ID of the owner'),
       'owner': fields.Nested(user_model, description='Owner of the place'),
       'amenities': fields.List(fields.Nested(amenity_model), description='List of amenities')
   })

   facade = HBnBFacade()

   @api.route('/')
   class PlaceList(Resource):
       @api.expect(place_model)
       @api.response(201, 'Place successfully created')
       @api.response(400, 'Invalid input data')
       def post(self):
           """Register a new place"""
           # Placeholder for the logic to register a new place
           pass

       @api.response(200, 'List of places retrieved successfully')
       def get(self):
           """Retrieve a list of all places"""
           # Placeholder for logic to return a list of all places
           pass   

   @api.route('/<place_id>')
   class PlaceResource(Resource):
       @api.response(200, 'Place details retrieved successfully')
       @api.response(404, 'Place not found')
       def get(self, place_id):
           """Get place details by ID"""
           # Placeholder for the logic to retrieve a place by ID, including associated owner and amenities
           pass

       @api.expect(place_model)
       @api.response(200, 'Place updated successfully')
       @api.response(404, 'Place not found')
       @api.response(400, 'Invalid input data')
       def put(self, place_id):
           """Update a place's information"""
           # Placeholder for the logic to update a place by ID
           pass
   ```

   **Explanation:**
   - The `POST` endpoint registers a new place, while `GET` and `PUT` manage place retrieval and updates. The `GET /api/v1/places/<place_id>` endpoint includes associated owner and amenities for the place.
   - You are responsible for implementing the logic for handling relationships between places, owners, and amenities.

2. **Implement the Place Management Logic in the Business Logic Layer**
   - In the `models/place.py` file, ensure that the `Place` class handles relationships with other entities and performs attribute validation.

   **Placeholders for Facade Methods:**
   ```python
   def create_place(self, place_data):
       # Placeholder for logic to create a place, including validation for price, latitude, and longitude
       pass

   def get_place(self, place_id):
       # Placeholder for logic to retrieve a place by ID, including associated owner and amenities
       pass

   def get_all_places(self):
       # Placeholder for logic to retrieve all places
       pass

   def update_place(self, place_id, place_data):
       # Placeholder for logic to update a place
       pass
   ```

   **Explanation:**
   - The `create_place`, `get_place`, `get_all_places`, and `update_place` methods handle place creation, retrieval, and updates. Ensure that validation for attributes like `price`, `latitude`, and `longitude` is implemented. Consider using custom setter methods or raising exceptions to handle invalid data.

3. **Validation of Place Attributes**

   When implementing validation for attributes like `price`, `latitude`, and `longitude`, consider the following:
   - `price` should be a non-negative float.
   - `latitude` should be between -90 and 90.
   - `longitude` should be between -180 and 180.

   Implement these validations using property setters in the `Place` class, raising appropriate exceptions for invalid values.

4. **Input and Output Formats, Status Codes**

   Ensure that the input format, output format, and status codes are consistent and clearly defined for each endpoint:

   - **POST /api/v1/places/** (Register a new place)
     - **Input:**
       ```json
       {
           "title": "Cozy Apartment",
           "description": "A nice place to stay",
           "price": 100.0,
           "latitude": 37.7749,
           "longitude": -122.4194,
           "owner_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
       }
       ```
     - **Output:**
       ```json
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "message": "Place created successfully"
       }
       ```
     - **Status Codes:**
       - `201 Created`: When the place is successfully created.
       - `400 Bad Request`: If input data is invalid.

   - **GET /api/v1/places/** (Retrieve all places)
     - **Output:**
       ```json
       [
          {
              "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
              "title": "Cozy Apartment",
              "latitude": 37.7749,
              "longitude": -122.4194
          },
          ...
       ]
       ```
     - **Status Codes:**
       - `200 OK`: List of places retrieved successfully.

   - **GET /api/v1/places/<place_id>** (Retrieve place details)
     - **Output:**
       ```json
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "title": "Cozy Apartment",
           "description": "A nice place to stay",
           "latitude": 37.7749,
           "longitude": -122.4194,
           "owner": {
               "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
               "first_name": "John",
               "last_name": "Doe",
               "email": "john.doe@example.com"
           },
           "amenities": [
               {
                   "id": "4fa85f64-5717-4562-b3fc-2c963f66afa6",
                   "name": "Wi-Fi"
               },
               {
                   "id": "5fa85f64-5717-4562-b3fc-2c963f66afa6",
                   "name": "Air Conditioning"
               }
           ]
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the place and its associated owner and amenities are successfully retrieved.
       - `404 Not Found`: If the place does not exist.

   - **PUT /api/v1/places/<place_id>** (Update a place’s information)
     - **Input:**
       ```json
       {
           "title": "Luxury Condo",
           "description": "An upscale place to stay",
           "price": 200.0
       }
       ```
     - **Output:**
       ```json
       {
           "message": "Place updated successfully"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the place is successfully updated.
       - `404 Not Found`: If the place does not exist.
       - `400 Bad Request`: If input data is invalid.

5. **Testing the Endpoints**
   - Once the endpoints are implemented, use tools like Postman or cURL to test each operation:
     - **POST**: Register a new place.
     - **GET**: Retrieve a place’s details using its ID, including associated owner and amenities.
     - **PUT**: Update a place’s information.

   **Example Test Using cURL:**
   ```bash
   curl -X GET "http://127.0.0.1:5000/api/v1/places/<place_id>"
   ```

   **Expected Response:**
   ```json
   {
       "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
       "title": "Cozy Apartment",
       "description": "A nice place to stay",
       "latitude": 37.7749,
       "longitude": -122.4194,
       "owner": {
           "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
           "first_name": "John",
           "last_name": "Doe",
           "email": "john.doe@example.com"
       },
       "amenities": [
           {
               "id": "4fa85f64-5717-4562-b3fc-2c963f66afa6",
               "name": "Wi-Fi"
           },
           {
               "id": "5fa85f64-5717-4562-b3fc-2c963f66afa6",
               "name": "Air Conditioning"
           }
       ]
   }
   ```

6. **Document the Place Endpoints**
   - Update the documentation to describe each endpoint:
     - Path, HTTP method, expected payload, and response.
     - How to handle errors (e.g., place not found).

### Resources

1. **Flask-RESTx Documentation:** [https://flask-restx.readthedocs.io/](https://flask-restx.readthedocs.io/)
2. **Testing REST APIs with cURL:** [https://everything.curl.dev/](https://everything.curl.dev/)
3. **Designing RESTful APIs:** [https://restfulapi.net/](https://restfulapi.net/)

### Expected Outcome

By the end of this task, you should have implemented the core place management endpoints, including the ability to create, read, and update places. The `DELETE` operation will not be implemented for places in this part. You will have handled the relationships between places, owners, and amenities, including validating specific attributes like price, latitude, and longitude. The functionality should be documented and tested, ensuring smooth operation within the HBnB application.
