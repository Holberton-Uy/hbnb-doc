### Task 2: Implement Core Business Logic Classes

#### Objective
Implement the core business logic classes that define the entities of the HBnB application, including the necessary attributes, methods, and relationships. This task focuses on creating the fundamental models (User, Place, Review, and Amenity) while considering the design choices made by students in Part 1. 

#### Context
In Part 1, students designed the Business Logic layer, including defining entities and relationships. This task requires you to implement those designs while adhering to best practices for modular, maintainable code. You may have already created base classes with common attributes (e.g., `id`, `created_at`, and `updated_at`) to be inherited by concrete classes such as `User`, `Place`, `Review`, and `Amenity`.

In this task, you will:
1. Implement the classes based on your Part 1 design.
2. Ensure relationships between entities are correctly implemented.
3. Handle attribute validation and updates according to the requirements.

#### Instructions

1. **Implement the Core Classes**
   - In the `models/` directory, implement the classes defined in your design:
     - `user.py`
     - `place.py`
     - `review.py`
     - `amenity.py`
   - If you have created base classes in Part 1 (e.g., a base class for shared attributes like `id`, `created_at`, and `updated_at`), ensure that your entity classes inherit from them.

2. **Implementing UUID, Created_at, and Updated_at Attributes**
   - Each class should include:
     - A UUID identifier for each instance (`id = uuid.uuid4()`).
     - Timestamps for creation (`created_at`) and modification (`updated_at`).
     - The `created_at` timestamp should be set when an object is created, and the `updated_at` timestamp should be updated every time the object is modified.
   - Example base class for handling common attributes:
   
     ```python
     import uuid
     from datetime import datetime

     class BaseModel:
         def __init__(self):
             self.id = str(uuid.uuid4())
             self.created_at = datetime.now()
             self.updated_at = datetime.now()

         def save(self):
             """Update the updated_at timestamp whenever the object is modified"""
             self.updated_at = datetime.now()
     ```

   - In this example we store the `UUID` generated as a `String` to avoid problems when retrieving from the Memory Repository.
   - Ensure that the `id` is returned by the API, but `created_at` and `updated_at` should **not** be returned, as they are for internal audit purposes only.

3. **Class Implementation Guidelines**
   - Each class should include the following attributes, with appropriate types and value restrictions:

   **User Class:**
   
   - `id` (UUID): Unique identifier for each user.
   - `first_name` (String): The first name of the user. Required, maximum length of 50 characters.
   - `last_name` (String): The last name of the user. Required, maximum length of 50 characters.
   - `email` (String): The email address of the user. Required, must be unique, and should follow standard email format validation.
   - `is_admin` (Boolean): Indicates whether the user has administrative privileges. Defaults to `False`.
   - `created_at` (DateTime): Timestamp when the user is created.
   - `updated_at` (DateTime): Timestamp when the user is last updated.

   **Place Class:**
   
   - `id` (UUID): Unique identifier for each place.
   - `title` (String): The title of the place. Required, maximum length of 100 characters.
   - `description` (String): Detailed description of the place. Optional.
   - `price` (Float): The price per night for the place. Must be a positive value.
   - `latitude` (Float): Latitude coordinate for the place location. Must be within the range of -90.0 to 90.0.
   - `longitude` (Float): Longitude coordinate for the place location. Must be within the range of -180.0 to 180.0.
   - `owner_id` (UUID): References the `User` who owns the place. This should be validated to ensure the owner exists.
   - `created_at` (DateTime): Timestamp when the place is created.
   - `updated_at` (DateTime): Timestamp when the place is last updated.

   **Review Class:**
   
   - `id` (UUID): Unique identifier for each review.
   - `text` (String): The content of the review. Required.
   - `rating` (Integer): Rating given to the place, must be between 1 and 5.
   - `place_id` (UUID): References the `Place` being reviewed. Must be validated to ensure the place exists.
   - `user_id` (UUID): References the `User` who wrote the review. Must be validated to ensure the user exists.
   - `created_at` (DateTime): Timestamp when the review is created.
   - `updated_at` (DateTime): Timestamp when the review is last updated.

   **Amenity Class:**
   
   - `id` (UUID): Unique identifier for each amenity.
   - `name` (String): The name of the amenity (e.g., "Wi-Fi", "Parking"). Required, maximum length of 50 characters.
   - `created_at` (DateTime): Timestamp when the amenity is created.
   - `updated_at` (DateTime): Timestamp when the amenity is last updated.

   - Methods should be implemented to handle core operations, such as creating, updating, and retrieving instances. For instance, `save` methods to update timestamps and validate input data according to the constraints listed.

5. **Implement Relationships Between Entities**
   - Define relationships between the classes as follows:

   **User and Place:**
   
   - A `User` can own multiple `Place` instances (`one-to-many` relationship).
   - The `Place` class should include an attribute `owner_id`, referencing the `User` who owns it.

   **Place and Review:**
   
   - A `Place` can have multiple `Review` instances (`one-to-many` relationship).
   - The `Review` class should include attributes `place_id` and `user_id`, referencing the `Place` being reviewed and the `User` who wrote the review, respectively.

   **Place and Amenity:**
   
   - A `Place` can have multiple `Amenity` instances (`many-to-many` relationship).
   - This relationship can be represented using a list of amenities within the `Place` class. For simplicity, in-memory storage or a list of amenity IDs can be used.

   - Example of implementing relationships:
   
     ```python
     class Place(BaseModel):
         def __init__(self, title, description, price, latitude, longitude, owner_id):
             super().__init__()
             self.title = title
             self.description = description
             self.price = price
             self.latitude = latitude
             self.longitude = longitude
             self.owner_id = owner_id
             self.reviews = []  # List to store related reviews
             self.amenities = []  # List to store related amenities

         def add_review(self, review):
             """Add a review to the place."""
             self.reviews.append(review)

         def add_amenity(self, amenity):
             """Add an amenity to the place."""
             self.amenities.append(amenity)
     ```

   - Implement methods for managing these relationships, like adding a review to a place, or listing amenities associated with a place. Ensure that these operations validate the existence of related entities to maintain data integrity.

7. **Test the Core Classes Independently**
   - Before moving on to the API implementation, write simple tests to validate that the classes are functioning as expected.
   - Ensure that relationships between entities (e.g., adding a review to a place) work correctly.

   **Example Tests**

  Here’s a basic guide on how to test your implementation:

  1. **Testing the User Class:**

       ```python
       from models.user import User
   
       def test_user_creation():
           user = User(first_name="John", last_name="Doe", email="john.doe@example.com")
           assert user.first_name == "John"
           assert user.last_name == "Doe"
           assert user.email == "john.doe@example.com"
           assert user.is_admin is False  # Default value
           print("User creation test passed!")
   
       test_user_creation()
       ```

  2. **Testing the Place Class with Relationships:**

       ```python
       from models.place import Place
       from models.user import User
       from models.review import Review
   
       def test_place_creation():
           owner = User(first_name="Alice", last_name="Smith", email="alice.smith@example.com")
           place = Place(title="Cozy Apartment", description="A nice place to stay", price=100, latitude=37.7749, longitude=-122.4194, owner_id=owner.id)
   
           # Adding a review
           review = Review(text="Great stay!", rating=5, place_id=place.id, user_id=owner.id)
           place.add_review(review)
   
           assert place.title == "Cozy Apartment"
           assert place.price == 100
           assert len(place.reviews) == 1
           assert place.reviews[0].text == "Great stay!"
           print("Place creation and relationship test passed!")
   
       test_place_creation()
       ```

  3. **Testing the Amenity Class:**

       ```python
       from models.amenity import Amenity
   
       def test_amenity_creation():
           amenity = Amenity(name="Wi-Fi")
           assert amenity.name == "Wi-Fi"
           print("Amenity creation test passed!")
   
       test_amenity_creation()
       ```

6. **Document the Implementation**
   - Update the `README.md` file to include information about the Business Logic layer, describing the entities and their responsibilities.
   - Include examples of how the classes and methods can be used.

#### Resources

1. **Python OOP Basics:** [https://realpython.com/python3-object-oriented-programming/](https://realpython.com/python3-object-oriented-programming/)
2. **Designing Classes and Relationships:** [https://docs.python.org/3/tutorial/classes.html](https://docs.python.org/3/tutorial/classes.html)
3. **Why You Should Use UUIDs:** [https://datatracker.ietf.org/doc/html/rfc4122](https://datatracker.ietf.org/doc/html/rfc4122)

#### Expected Outcome

By the end of this task, you should have fully implemented core business logic classes (User, Place, Review, Amenity) with the appropriate attributes, methods, and relationships. With these components in place, you will be ready to proceed to implementing the API endpoints in the next task. The implemented classes should support the necessary validation, relationships, and data integrity checks required for the application’s core functionality. Additionally, the relationships between entities should be fully operational, allowing seamless interactions like linking reviews to places or associating amenities with places.

With this solid foundation in place, the business logic will be prepared for further integration with the Presentation and Persistence layers in subsequent tasks.

