### Task 4: Implement the Amenity Endpoints

#### Objective
Implement the API endpoints required for managing amenities in the HBnB application. This task involves setting up the endpoints to handle CRUD operations (Create, Read, Update) for amenities, while ensuring integration with the Business Logic layer via the Facade pattern. The `DELETE` operation will not be implemented for amenities in this part of the project.

In this task, you will:
1. Set up the `POST`, `GET`, and `PUT` endpoints for managing amenities.
2. Implement the necessary logic for handling amenity-related operations in the Business Logic layer.
3. Integrate the Presentation layer (API) and Business Logic layer through the Facade.

#### Instructions

1. **Set Up the Amenity Endpoints in the Presentation Layer (API)**
   - In the `api/v1/amenities.py` file, define the following endpoints:
     - `POST /api/v1/amenities/`: Register a new amenity.
     - `GET /api/v1/amenities/`: Return a list of all amenities.
     - `GET /api/v1/amenities/<amenity_id>`: Retrieve details of a specific amenity.
     - `PUT /api/v1/amenities/<amenity_id>`: Update amenity information.

   Your task is to define the routes and create the skeleton methods for these endpoints. Use the placeholders provided below to get started.

   **Placeholders:**
   ```python
   from flask_restx import Namespace, Resource, fields
   from app.services.facade import HBnBFacade

   api = Namespace('amenities', description='Amenity operations')

   # Define the amenity model for input validation and documentation
   amenity_model = api.model('Amenity', {
       'name': fields.String(required=True, description='Name of the amenity')
   })

   facade = HBnBFacade()

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

   **Explanation:**
   - The `POST` endpoint handles the creation of a new amenity, while the `GET` endpoints manage retrieval, both for a single amenity and a list of all amenities. The `PUT` endpoint is responsible for updating an existing amenity’s details.
   - The placeholders give you a foundation to build on, while you will need to implement the logic based on previous examples like the user registration task.

2. **Implement the Amenity Management Logic in the Business Logic Layer**
   - In the `models/amenity.py` file, the `Amenity` class should have already been implemented in Task 2. Make sure that this class is capable of handling updates and storing data correctly.

   **Placeholders for Facade Methods:**
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

   **Explanation:**
   - The `create_amenity`, `get_amenity`, `get_all_amenities`, and `update_amenity` methods manage the creation, retrieval, and updating of amenities within the Business Logic layer. You will need to fill in the logic that handles interactions with the repository and implements necessary validation.

3. **Input and Output Formats, Status Codes**

   For each endpoint, ensure that the input format, output format, and status codes are consistent and clearly defined:

   - **POST /api/v1/amenities/** (Register a new amenity)
     - **Input:**
       ```json
       {
           "name": "Wi-Fi"
       }
       ```
     - **Output:**
       ```json
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "message": "Amenity created successfully"
       }
       ```
     - **Status Codes:**
       - `201 Created`: When the amenity is successfully created.
       - `400 Bad Request`: If input data is invalid.

   - **GET /api/v1/amenities/** (Retrieve all amenities)
     - **Output:**
       ```json
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
       ```
     - **Status Codes:**
       - `200 OK`: List of amenities retrieved successfully.

   - **GET /api/v1/amenities/<amenity_id>** (Retrieve an amenity’s details)
     - **Output:**
       ```json
       {
           "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
           "name": "Wi-Fi"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the amenity is successfully retrieved.
       - `404 Not Found`: If the amenity does not exist.

   - **PUT /api/v1/amenities/<amenity_id>** (Update an amenity’s information)
     - **Input:**
       ```json
       {
           "name": "Air Conditioning"
       }
       ```
     - **Output:**
       ```json
       {
           "message": "Amenity updated successfully"
       }
       ```
     - **Status Codes:**
       - `200 OK`: When the amenity is successfully updated.
       - `404 Not Found`: If the amenity does not exist.
       - `400 Bad Request`: If input data is invalid.

4. **Testing the Endpoints**
   - Once the endpoints are implemented, use tools like Postman or cURL to test each operation:
     - **POST**: Register a new amenity.
     - **GET**: Retrieve an amenity’s details using its ID.
     - **PUT**: Update an amenity’s information.

   **Example Test Using cURL:**
   ```bash
   curl -X GET "http://127.0.0.1:5000/api/v1/amenities/<amenity_id>"
   ```

   **Expected Response:**
   ```json
   {
       "id": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
       "name": "Wi-Fi"
   }
   ```

5. **Document the Amenity Endpoints**
   - Update the documentation to describe each endpoint:
     - Path, HTTP method, expected payload, and response.
     - How to handle errors (e.g., amenity not found).

#### Resources

1. **Flask-RESTx Documentation:** [https://flask-restx.readthedocs.io/](https://flask-restx.readthedocs.io/)
2. **Testing REST APIs with cURL:** [https://everything.curl.dev/](https://everything.curl.dev/)
3. **Designing RESTful APIs:** [https://restfulapi.net/](https://restfulapi.net/)

#### Expected Outcome

By the end of this task, you should have fully implemented the core amenity management endpoints, including the ability to create, read, and update amenities. The `DELETE` operation will not be implemented for amenities in this part. The provided placeholders should guide you in implementing the logic based on the example provided for user registration. The functionality should be documented and tested, ensuring that all amenity-related operations are handled smoothly within the HBnB application.
