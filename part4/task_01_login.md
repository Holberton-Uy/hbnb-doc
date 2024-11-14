#### Objectives

- Implement login functionality using the back-end API.
- Store the JWT token returned by the API in a cookie for session management.

#### Requirements

- Use the existing login form provided in [login.html](./base_files/login.html).
- Make an AJAX request to the login endpoint of your API when the user submits the login form.
- If the login is successful, **store the JWT token in a cookie**.
- Redirect the user to the main page (`index.html`) after a successful login.
- Display an error message if the login fails.

#### Instructions

1. **Setup Event Listener for Login Form**:
   - Add an **event listener** to the login form to handle the form submission.
   - Use `preventDefault` to prevent the default form submission behavior.

2. **Make AJAX Request to API**:
   - Use the Fetch API to send a POST request to the login endpoint with the email and password entered by the user.
   - Set the `Content-Type` header to `application/json`.
   - Send the email and password in the request body as a JSON object.

3. **Handle API Response**:
   - If the login is successful, store the returned JWT token in a cookie.
   - Redirect the user to the main page (`index.html`).
   - If the login fails, display an error message to the user.

4. **Example Guidance**:

**scripts.js**

- Add an event listener for the form submission:

  ```javascript
  document.addEventListener('DOMContentLoaded', () => {
      const loginForm = document.getElementById('login-form');

      if (loginForm) {
          loginForm.addEventListener('submit', async (event) => {
              event.preventDefault();
              // Your code to handle form submission
          });
      }
  });
  ```

- Make the AJAX request to the API:

  ```javascript
  async function loginUser(email, password) {
      const response = await fetch('https://your-api-url/login', {
          method: 'POST',
          headers: {
              'Content-Type': 'application/json'
          },
          body: JSON.stringify({ email, password })
      });
      // Handle the response
  }
  ```

- Handle the API response and store the token in a cookie:

  ```javascript
  if (response.ok) {
      const data = await response.json();
      document.cookie = `token=${data.access_token}; path=/`;
      window.location.href = 'index.html';
  } else {
      alert('Login failed: ' + response.statusText);
  }
  ```

5. **Testing**:
   - Test the login functionality with valid and invalid credentials to ensure it works as expected.
   - Verify that the JWT token is stored in the cookie after a successful login.
   - Ensure that the user is redirected to the main page after login.

> [!WARNING]
> When testing your client against yout API you'll probably get a Cross-Origin Resource Sharing (CORS) error. You'll need to modify your API code to allow your client to fetch data from the API.
> Read [this article](https://medium.com/@mterrano1/cors-in-a-flask-api-38051388f8cc) for a depper understanding about CORS and how to configure your Flask API

#### Resources

- [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [Handling Cookies in JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
- [HTML5 Form Validation](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation)
