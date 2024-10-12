#### Objectives

- Implement the detailed view of a place.
- Fetch place details from the API using the place ID.
- Display detailed information about the place, including name, description, price, amenities and reviews.
- If the user is authenticated, provide access to the form for adding a review.

#### Requirements

- Use the provided HTML structure in `place.html` to display the detailed information of a place.
- Make an AJAX request to the API to fetch the details of the selected place.
- Populate the place details dynamically using JavaScript.
- Show the add review form only if the user is authenticated.

#### Instructions

1. **Get Place ID from URL**:
   - Extract the place ID from the URL query parameters.
   - Tip: Use `window.location.search` to get the query string.

2. **Check User Authentication**:
   - On page load, check if the user is authenticated by verifying the presence of the JWT token in cookies.
   - Store the token in a variable for later use in API requests.

3. **Fetch Place Details**:
   - Use the Fetch API to send a GET request to the endpoint that returns the details of the place.
   - Ensure the request includes the JWT token for authentication if available.
   - Tip: Include the token in the `Authorization` header of your request.

4. **Populate Place Details**:
   - Dynamically create HTML elements to display the place's detailed information (e.g., name, description, price, amenities and reviews).
   - Append these elements to the `#place-details` section.

5. **Show Add Review Form**:
   - If the user is authenticated, display the add review form.
   - Hide the form if the user is not authenticated.

#### Example Guidance

**scripts.js**

- **Get place ID from URL**:
  - Create a function to extract the place ID from the query parameters.

  ```javascript
  function getPlaceIdFromURL() {
      // Extract the place ID from window.location.search
      // Your code here
  }
  ```

- **Check user authentication**:
  - Create a function to check for the JWT token in cookies and store it in a variable.

  ```javascript
  function checkAuthentication() {
      const token = getCookie('token');
      const addReviewSection = document.getElementById('add-review');

      if (!token) {
          addReviewSection.style.display = 'none';
      } else {
          addReviewSection.style.display = 'block';
          // Store the token for later use
          fetchPlaceDetails(token, placeId);
      }
  }

  function getCookie(name) {
      // Function to get a cookie value by its name
      // Your code here
  }
  ```

- **Fetch place details**:
  - Use the Fetch API to get the details of the place and handle the response.

  ```javascript
  async function fetchPlaceDetails(token, placeId) {
      // Make a GET request to fetch place details
      // Include the token in the Authorization header
      // Handle the response and pass the data to displayPlaceDetails function
  }
  ```

- **Populate place details**:
  - Create HTML elements for the place details and append them to the `#place-details` section.

  ```javascript
  function displayPlaceDetails(place) {
      // Clear the current content of the place details section
      // Create elements to display the place details (name, description, price, amenities and reviews)
      // Append the created elements to the place details section
  }
  ```

6. **Testing**:
   - Test the functionality by navigating to the place details page and verifying the displayed information.
   - Ensure that the add review form appears only when the user is authenticated.

#### Resources

- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Handling Cookies in JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [DOM Manipulation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)