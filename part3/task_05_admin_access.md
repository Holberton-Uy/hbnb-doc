### Task: Implement Administrator Access Endpoints

#### Objective
Restrict access to specific API endpoints so that only users with administrative privileges can perform certain actions. These actions include creating new users, modifying any user's details (including email and password), and adding or modifying amenities. Additionally, administrators can perform the same tasks as authenticated users without being restricted by ownership of places and reviews.

#### Context
Role-based access control (RBAC) is crucial in API security. Administrators have the highest level of privileges, and this task will allow them to bypass restrictions that regular users face. This includes the ability to manage any user or resource in the system. 

In this task, you will:
1. Implement logic for restricting access to specific endpoints based on the user's role (`is_admin`).
2. Ensure that administrators can manage user accounts, including creating and modifying user details.
3. Allow administrators to bypass ownership restrictions for places and reviews.

---

#### Instructions

1. **Define Administrator Permissions**
   The following endpoints will be restricted to administrators only:
   - **POST /api/v1/users/**: Create a new user.
   - **PUT /api/v1/users/<user_id>**: Modify a user's details, including email and password.
   - **POST /api/v1/amenities/**: Add a new amenity.
   - **PUT /api/v1/amenities/<amenity_id>**: Modify the details of an amenity.

   Administrators will also be able to:
   - Modify or delete any place or review, bypassing the ownership restrictions that regular users face.

2. **Add Role-Based Access Control (RBAC)**
   Use the `@jwt_required()` decorator to ensure that only authenticated users can access these endpoints. The `get_jwt()` function retrieves the JWT claims, allowing you to check the `is_admin` flag.

   **Example for Checking Admin Status:**
   ```python
   from flask_restx import Namespace, Resource
   from flask_jwt_extended import jwt_required, get_jwt_identity, get_jwt

   api = Namespace('admin', description='Admin operations')

   @api.route('/users/<user_id>')
   class AdminUserResource(Resource):
       @jwt_required()
       def put(self, user_id):
           current_user = get_jwt_identity()
           
           # If 'is_admin' is part of the identity payload
           if not current_user.get('is_admin'):
               return {'error': 'Admin privileges required'}, 403

           data = request.json
           email = data.get('email')

           if email:
               # Check if email is already in use
               existing_user = facade.get_user_by_email(email)
               if existing_user and existing_user.id != user_id:
                   return {'error': 'Email is already in use'}, 400

           # Logic to update user details, including email and password
           pass
   ```

3. **Implement Endpoint Logic**

   - **Create User (POST /api/v1/users/):**
     Administrators can create new users. The email must be unique.
     ```python
     @api.route('/users/')
     class AdminUserCreate(Resource):
         @jwt_required()
         def post(self):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             user_data = request.json
             email = user_data.get('email')

             # Check if email is already in use
             if facade.get_user_by_email(email):
                 return {'error': 'Email already registered'}, 400

             # Logic to create a new user
             pass
     ```

   - **Modify User (PUT /api/v1/users/<user_id>):**
     Administrators can modify any user, including changing the email and password, but must ensure that the email is not duplicated.
     ```python
     @api.route('/users/<user_id>')
     class AdminUserModify(Resource):
         @jwt_required()
         def put(self, user_id):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             data = request.json
             email = data.get('email')

             # Ensure email uniqueness
             if email:
                 existing_user = facade.get_user_by_email(email)
                 if existing_user and existing_user.id != user_id:
                     return {'error': 'Email already in use'}, 400

             # Logic to update user details
             pass
     ```

   - **Add Amenity (POST /api/v1/amenities/):**
     ```python
     @api.route('/amenities/')
     class AdminAmenityCreate(Resource):
         @jwt_required()
         def post(self):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             # Logic to create a new amenity
             pass
     ```

   - **Modify Amenity (PUT /api/v1/amenities/<amenity_id>):**
     ```python
     @api.route('/amenities/<amenity_id>')
     class AdminAmenityModify(Resource):
         @jwt_required()
         def put(self, amenity_id):
             current_user = get_jwt_identity()
             if not current_user.get('is_admin'):
                 return {'error': 'Admin privileges required'}, 403

             # Logic to update an amenity
             pass
     ```

4. **Allow Admins to Bypass Ownership Restrictions**
   When an admin attempts to modify or delete a place or review, ownership checks will be bypassed.

   **Example for Modifying a Place (PUT /api/v1/places/<place_id>):**
   ```python
   @api.route('/places/<place_id>')
   class AdminPlaceModify(Resource):
       @jwt_required()
       def put(self, place_id):
           current_user = get_jwt_identity()

           # Set is_admin default to False if not exists
           is_admin = current_user.get('is_admin', False)
           user_id = current_user.get('id')

           place = facade.get_place(place_id)
           if not is_admin and place.owner_id != user_id:
               return {'error': 'Unauthorized action'}, 403

           # Logic to update the place
           pass
   ```

5. **Test the Admin Endpoints**

    > [!IMPORTANT] Unauthorized action
    > At this point, you should have realized that you'll need a user with administrative permissions to test the admin endpoints. 
    > Discuss different strategies with your team to overcome this problem.

   - **Create a New User as an Admin:**
     ```bash
     curl -X POST "http://127.0.0.1:5000/api/v1/users/" -d '{"email": "newuser@example.com", "first_name": "Admin", "last_name": "User"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Modify Another User’s Data as an Admin:**
     ```bash
     curl -X PUT "http://127.0.0.1:5000/api/v1/users/<user_id>" -d '{"email": "updatedemail@example.com"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Add a New Amenity as an Admin:**
     ```bash
     curl -X POST "http://127.0.0.1:5000/api/v1/amenities/" -d '{"name": "Swimming Pool"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

   - **Modify an Amenity as an Admin:**
     ```bash
     curl -X PUT "http://127.0.0.1:5000/api/v1/amenities/<amenity_id>" -d '{"name": "Updated Amenity"}' -H "Authorization: Bearer <admin_token>" -H "Content-Type: application/json"
     ```

---

#### Resources

1. **Flask-JWT-Extended Documentation:** [Flask-JWT-Extended](https://flask-jwt-extended.readthedocs.io/en/stable/)
2. **Bcrypt Documentation:** [Flask-Bcrypt](https://flask-bcrypt.readthedocs.io/en/latest/)

#### Expected Outcome
By the end of this task, admins will be able to:
- Create new users.
- Modify any user’s data, including email and password (with validation for unique emails).
- Add and modify amenities.
- Bypass ownership restrictions on places and reviews.
