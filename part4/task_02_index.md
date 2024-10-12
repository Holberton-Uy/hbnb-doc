#### Objectives

- Implement the main page to display a list of all places.
- Fetch places data from the API and implement client-side filtering based on price.
- Show the login link only if the user is not authenticated.

#### Requirements

- Use the provided HTML structure in `index.html` to display the list of places.
- Make an AJAX request to the API to fetch the list of places.
- Populate the places list dynamically using JavaScript.
- Implement a client-side filter to allow users to filter places by price without reloading the page.
- Show or hide the login link based on user authentication.

#### Instructions

1. **Check User Authentication**:
   - On page load, check if the user is authenticated by verifying the presence of the JWT token in cookies.
   - If the token is not found, show the login link.
   - If the token is found, hide the login link.
   - Tip: Use a function to get the value of a cookie by its name.

2. **Fetch Places Data**:
   - Use the Fetch API to send a GET request to the endpoint that returns the list of places.
   - Ensure the request includes the JWT token for authentication if available.
   - Tip: Include the token in the `Authorization` header of your request.

3. **Populate Places List**:
   - Dynamically create HTML elements to display each place's information (e.g., name, description, location).
   - Append these elements to the `#places-list` section.
   - Tip: Use `document.createElement` and `element.innerHTML` to build the place elements.

4. **Implement Client-Side Filtering**:
   - Add an event listener to the price filter dropdown.
   - Filter the displayed places based on the selected price.
   - Ensure the filtering works without reloading the page.
   - Tip: Use `element.style.display` to show or hide places based on the filter.

#### Example Guidance

**scripts.js**

- **Check user authentication**:
  - Create a function to check for the JWT token in cookies and control the visibility of the login link.
 
  ```
  function checkAuthentication() {
      const token = getCookie('token');
      const loginLink = document.getElementById('login-link');

      if (!token) {
          loginLink.style.display = 'block';
      } else {
          loginLink.style.display = 'none';
          // Fetch places data if the user is authenticated
          fetchPlaces(token);
      }
  }
  function getCookie(name) {
      // Function to get a cookie value by its name
      // Your code here
  }
  ```

- **Fetch places data**:
  - Use the Fetch API to get the list of places and handle the response.

  ```
  async function fetchPlaces(token) {
      // Make a GET request to fetch places data
      // Include the token in the Authorization header
      // Handle the response and pass the data to displayPlaces function
  }
  ```

- **Populate places list**:

  - Create HTML elements for each place and append them to the `#places-list`.
 
  ```
  function displayPlaces(places) {
      // Clear the current content of the places list
      // Iterate over the places data
      // For each place, create a div element and set its content
      // Append the created element to the places list
  }
  ```

- **Implement client-side filtering**:
  - Add an event listener to the price filter dropdown to filter places based on the selected price.
  - The filter will set the top price for the places to be shown.
  - The dropdown must be loaded with the following options:
    - 10
    - 50
    - 100
    - All
  

  ```
  document.getElementById('price-filter').addEventListener('change', (event) => {
      // Get the selected price value
      // Iterate over the places and show/hide them based on the selected price
  });
  ```

6. **Testing**:
   - Test the functionality by logging in and viewing the list of places.
   - Verify that the client-side filter works as expected.
   - Ensure the login link appears only when the user is not authenticated.

#### Resources

- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Handling Cookies in JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [DOM Manipulation](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction)