### Task: Generate Database Diagrams

#### Objective
Create Entity-Relationship (ER) diagrams to visually represent the structure of the database schema for the HBnB project using **Mermaid.js**. This task will help ensure that the database schema is accurately reflected and consistent with previous parts of the project. Students will use Mermaid.js to visualize tables and their relationships in a readable format.

#### Context
Visualizing database relationships is crucial for understanding the connections between entities. ER diagrams provide a high-level overview of the database's structure and serve as an essential reference for further development or debugging. Mermaid.js is a tool that allows you to create these diagrams in markdown-like syntax, making it easy to integrate them into documentation and collaboration platforms like GitHub or GitLab.

In this task, students will:
1. Learn to use Mermaid.js to create ER diagrams.
2. Generate diagrams that represent the `User`, `Place`, `Review`, `Amenity`, and `Place_Amenity` tables, along with their relationships.
3. Ensure consistency in the schema visualization, using Mermaid.js as the primary tool for diagram generation.

---

### Instructions

1. **Learn the Mermaid.js Syntax for ER Diagrams**

   Mermaid.js allows the generation of diagrams through simple text-based syntax. In this case, we'll focus on creating **ER diagrams** to visualize the structure of a relational database.
   
   Below is a general example of Mermaid.js syntax for an ER diagram:
   
   ```mermaid
   erDiagram
       PERSON {
           int id
           string first_name
           string last_name
           date birth_date
       }

       ADDRESS {
           int id
           string street
           string city
           string postal_code
       }

       COMPANY {
           int id
           string name
       }

       PERSON ||--o{ ADDRESS : has
       COMPANY ||--o{ PERSON : employs
   ```

   **Explanation**:
   - `PERSON`, `ADDRESS`, and `COMPANY` are entities (tables) in the database.
   - The `||--o{` notation is used to represent relationships:
     - `||` signifies a one-to-one relationship.
     - `o{` signifies a one-to-many relationship.
   - The attributes inside each entity represent the columns in the table, with types such as `int`, `string`, and `date`.

2. **Generate the Database Diagram for HBnB**

   After learning the syntax, create an ER diagram representing the core entities from your HBnB project: `User`, `Place`, `Review`, `Amenity`, and `Place_Amenity`.

   **Entities to include**:
   
   - **User**
     - Attributes: `id`, `first_name`, `last_name`, `email`, `password`, `is_admin`
   
   - **Place**
     - Attributes: `id`, `title`, `description`, `price`, `latitude`, `longitude`, `owner_id`
     - Relationships: `Place` has a foreign key `owner_id` referencing the `User` table.

   - **Review**
     - Attributes: `id`, `text`, `rating`, `user_id`, `place_id`
     - Relationships: `Review` has foreign keys `user_id` (references `User`) and `place_id` (references `Place`).
   
   - **Amenity**
     - Attributes: `id`, `name`
   
   - **Place_Amenity** (A join table to represent the many-to-many relationship between `Place` and `Amenity`)
     - Attributes: `place_id`, `amenity_id`
     - Relationships: 
       - `Place_Amenity` has two foreign keys:
         - `place_id` (references `Place`)
         - `amenity_id` (references `Amenity`)
   
3. **Generate and Export the Diagram**

   After writing the Mermaid.js code for the ER diagram:
   
   - Use an online editor like [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/) to visualize your diagram.
   - Export the diagram as an image (e.g., PNG or SVG) for documentation purposes.
   
   You can also integrate Mermaid diagrams into platforms such as GitHub by including the diagram code directly in your markdown files.

4. **Test Your Understanding**
   
   To ensure you understand how relationships work, modify the diagram to include potential new entities, such as a `Reservation` or `Booking` table, and how they might relate to `User` and `Place`.

---

#### Resources

- **Mermaid.js Documentation**: [Mermaid.js Official Docs](https://mermaid-js.github.io/mermaid/)
- **Live Mermaid.js Editor**: [Mermaid Live Editor](https://mermaid-js.github.io/mermaid-live-editor/)
- **SQLAlchemy Relationship Documentation**: [SQLAlchemy ORM Relationships](https://docs.sqlalchemy.org/en/20/orm/basic_relationships.html)

---

#### Expected Outcome

By the end of this task, you should have:

- Created an ER diagram in Mermaid.js that accurately represents the database schema for HBnB.
- Understood the various types of relationships (one-to-many, many-to-many) and how they are represented visually.
- Exported the diagram to be included in your project documentation.
