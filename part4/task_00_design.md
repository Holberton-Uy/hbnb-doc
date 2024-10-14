#### Objectives

- Complete the provided HTML and CSS files to match the given design specifications.
- Create the following pages:
  - Login Form
  - List of Places
  - Place Details
  - Add Review Form

#### Requirements

- Use the provided HTML and CSS files as a starting point.
- Follow the design specifications closely to achieve the intended look and feel.

#### Instructions

1. **Download the Provided Files**:
   - Obtain the HTML and CSS files provided as the starting point for this task.
   -> [Base Files](./base_files/)

2. **Complete the HTML Structure**:
   - Use semantic HTML5 elements to structure the content of each page.
   - Ensure the structure matches the design specifications provided below.

3. **Apply CSS Styles**:
   - Use the provided CSS file and add necessary styles to achieve the desired design.

4. **Pages to Complete**:
   - **Login Form**: Create a login form with fields for email and password.
   - **List of Places**: Design a page to display a list of places with basic information.
   - **Place Details**: Create a detailed view for a specific place, including detailed information.
   - **Add Review Form**: Design a form for adding a review to a place, accessible only to authenticated users.

#### Instructions for Styles and Structure

1. **Required Structure**:
   - **Header**:
     - Must include the application logo ([logo.png](./base_files/images/logo.png)) with the class `logo`.
     - Must include the login button or link with the class `login-button`.
   - **Footer**:
     - Must include text indicating all rights reserved.
   - **Navigation Bar**:
     - Must include relevant navigation links (e.g., `index.html` and `login.html`).

2. **Data to Display**:
   - **Index (index.html)**:
     - Display a list of places as "cards" using the class `place-card`.
     - Each card must include the name, the price per night and a "View Details" button with the class `details-button`.
   - **Place Details (place.html)**:
     - Display extended information about the place, including the host, price, description, and amenities using the classes `place-details` and `place-info`.
     - List reviews if they exist, each displayed as a card with the comment, user name, and rating using the class `review-card`.
     -  Include a button to navigate to the `add_review.html` page if the user is logged in.
     - Optional: Substitute the previous button with a form to add a new review if the user is logged in, using the classes `add-review` and `form`.


3. **Fixed Parameters**:
   - **Margin**: Use a margin of `20px` for place and review cards.
   - **Padding**: Use a padding of `10px` within place and review cards.
   - **Border**: Use a border of `1px solid #ddd` for place and review cards.
   - **Border Radius**: Use a border radius of `10px` for place and review cards.

4. **Flexible Parameters**:
   - **Color Palette**: Students can choose their color palette.
   - **Font**: Students can choose their font.
   - **Images**: Students can choose the images to use. Some sample images already provided with the base code.
   - **FavIcon**: Students can add a custom `favicon` or use the already provided `icon.png`.
 
> [!IMPORTANT]
> All pages MUST be valid on [W3C Validator page](https://validator.w3.org/).


#### Sample Design

![Index Page](./doc_images/img_index.png)

![Place Details](./doc_images/img_place.png)

![Review form](./doc_images/img_review.png)

![Place Details with review form](./doc_images/img_place_review.png)



---

#### Resources

- [HTML5 Documentation](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5)
- [CSS3 Documentation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3)
- [HTML Semantic Elements](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#Semantics_in_HTML)
