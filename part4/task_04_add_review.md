#### Objectives

- Implement the form to add a review for a place.
- Ensure only authenticated users can submit reviews.
- Redirect unauthenticated users to the index page.
- Send the review data to the API endpoint and handle the response.

#### Requirements

- Use the provided HTML structure in `add_review.html` to create the review form.
- Make an AJAX request to the API to submit the review data.
- If the user is not authenticated, redirect them to the index page.
- Display a success message upon successful submission and handle errors appropriately.

#### Instructions

1. **Check User Authentication**:
   - On page load, check if the user is authenticated by verifying the presence of the JWT token in cookies.
   - If the token is not found, redirect the user to the index page.
   - Store the token in a variable for later use in API requests.

2. **Get Place ID from URL**:
   - Extract the place ID from the URL query parameters.
   - Tip: Use `window.location.search` to get the query string.

3. **Setup Event Listener for Review Form**:
   - Add an event listener to the review form to handle the form submission.
   - Use `preventDefault` to prevent the default form submission behavior.

4. **Make AJAX Request to Submit Review**:
   - Use the Fetch API to send a POST request to the endpoint that submits the review data.
   - Include the JWT token in the `Authorization` header.
   - Send the review text and place ID in the request body as a JSON object.

5. **Handle API Response**:
   - If the submission is successful, display a success message and clear the form.
   - If the submission fails, display an error message to the user.

#### Example Guidance

**scripts.js**

- **Check user authentication**:
  - Create a function to check for the JWT token in cookies and redirect unauthenticated users.

  ```javascript
  function checkAuthentication() {
      const token = getCookie('token');
      if (!token) {
          window.location.href = 'index.html';
      }
      return token;
  }

  function getCookie(name) {
      // Function to get a cookie value by its name
      // Your code here
  }
  ```

- **Get place ID from URL**:
  - Create a function to extract the place ID from the query parameters.

  ```javascript
  function getPlaceIdFromURL() {
      // Extract the place ID from window.location.search
      // Your code here
  }
  ```

- **Setup event listener for review form**:
  - Add an event listener for the form submission to handle the review data.

  ```javascript
  document.addEventListener('DOMContentLoaded', () => {
      const reviewForm = document.getElementById('review-form');
      const token = checkAuthentication();
      const placeId = getPlaceIdFromURL();

      if (reviewForm) {
          reviewForm.addEventListener('submit', async (event) => {
              event.preventDefault();
              // Get review text from form
              // Make AJAX request to submit review
              // Handle the response
          });
      }
  });
  ```

- **Make AJAX request to submit review**:
  - Use the Fetch API to send a POST request with the review data.

  ```javascript
  async function submitReview(token, placeId, reviewText) {
      // Make a POST request to submit review data
      // Include the token in the Authorization header
      // Send placeId and reviewText in the request body
      // Handle the response
  }
  ```

- **Handle API response**:
  - Display a success message if the submission is successful and clear the form.
  - Display an error message if the submission fails.

  ```javascript
  function handleResponse(response) {
      if (response.ok) {
          alert('Review submitted successfully!');
          // Clear the form
      } else {
          alert('Failed to submit review');
      }
  }
  ```

6. **Testing**:
   - Test the functionality by submitting reviews for a place as an authenticated user.
   - Verify that unauthenticated users are redirected to the index page.
   - Ensure that success and error messages are displayed appropriately.

#### Resources

- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Handling Cookies in JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [DOM Manipulation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)
- [FormData API](https://developer.mozilla.org/en-US/docs/Web/API/FormData)