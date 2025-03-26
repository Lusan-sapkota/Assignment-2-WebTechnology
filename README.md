It covers the DOM significance with dynamic list manipulation, the many-to-many relationship between Teacher and Student, along with Flask-SQLAlchemy code.

---

# **Project README: DOM Manipulation and Flask-SQLAlchemy Many-to-Many Relationship**

## **1. DOM Manipulation with JavaScript**

### **Significance of the Document Object Model (DOM)**

The **Document Object Model (DOM)** is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style, and content. JavaScript interacts with the DOM to:
- Access elements, attributes, and text.
- Manipulate the structure of the page.
- Create dynamic and interactive user experiences.

### **Task Overview**

In this task, we demonstrate the manipulation of a dynamic list using JavaScript and the DOM. The user can add and remove items from an unordered list (`<ul>`) on an HTML page using buttons.

### **HTML and JavaScript Code Example**

Here is an example of how the DOM is manipulated to dynamically add and remove items from an unordered list when the user clicks the buttons:

#### **index.html**
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Manipulation Example</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    ul { list-style-type: disc; }
    button { margin: 5px; padding: 8px 12px; }
  </style>
</head>
<body>
  <h2>Dynamic List Manipulation</h2>
  <button id="addBtn">Add</button>
  <button id="removeBtn">Remove</button>
  <ul id="itemList">
    <li>Initial Item</li>
  </ul>

  <script>
    // Get references to DOM elements
    const addBtn = document.getElementById('addBtn');
    const removeBtn = document.getElementById('removeBtn');
    const itemList = document.getElementById('itemList');
    
    // Function to add a new list item
    function addListItem() {
      // Create a new list item element
      const newItem = document.createElement('li');
      newItem.textContent = 'New Item';
      // Append it to the unordered list
      itemList.appendChild(newItem);
    }
    
    // Function to remove the last list item
    function removeListItem() {
      // Check if there are list items to remove
      if (itemList.lastElementChild) {
        itemList.removeChild(itemList.lastElementChild);
      }
    }
    
    // Attach event listeners to buttons
    addBtn.addEventListener('click', addListItem);
    removeBtn.addEventListener('click', removeListItem);
  </script>
</body>
</html>
```

### **Explanation:**
- **HTML Structure:** Contains buttons (`Add` and `Remove`) and an unordered list (`<ul>`) to display the dynamic items.
- **JavaScript Functions:**
  - `addListItem`: Adds a new list item to the unordered list.
  - `removeListItem`: Removes the last list item if present.
- **Event Listeners:** When the buttons are clicked, respective functions are triggered.

---

## **2. Flask-SQLAlchemy: Teacher-Student Many-to-Many Relationship**

### **a. Diagram Illustrating the Relationship**

The many-to-many relationship between **Teacher** and **Student** is implemented using an association table. The diagram below shows how this relationship is established:

```
     Teacher
   +----------+
   | id       |
   | name     |
   | subject  |
   | email    |
   +----------+
         |
         |  many-to-many (via teacher_student)
         |
   +-----------------+
   | teacher_student |
   | teacher_id      |
   | student_id      |
   +-----------------+
         |
         |  many-to-many
         |
     Student
   +----------+
   | id       |
   | name     |
   | age      |
   | email    |
   +----------+
```

In this diagram:
- **Teacher** and **Student** models have a many-to-many relationship, which is managed by the `teacher_student` association table.
- Each teacher can teach many students, and each student can have many teachers.

### **b. Flask-SQLAlchemy Script (main.py)**

Below is the Python script to define the `Teacher` and `Student` models and their relationship using Flask-SQLAlchemy.

#### **main.py**

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
# Configure the SQLALCHEMY_DATABASE_URI with your database URL
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///school.db'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

db = SQLAlchemy(app)

# Association table for many-to-many relationship between Teacher and Student
teacher_student = db.Table('teacher_student',
    db.Column('teacher_id', db.Integer, db.ForeignKey('teacher.id'), primary_key=True),
    db.Column('student_id', db.Integer, db.ForeignKey('student.id'), primary_key=True)
)

class Teacher(db.Model):
    __tablename__ = 'teacher'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    subject = db.Column(db.String(50), nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    # Relationship with students through the association table
    students = db.relationship('Student', secondary=teacher_student, backref=db.backref('teachers', lazy='dynamic'))

    def __repr__(self):
        return f"<Teacher {self.name}>"

class Student(db.Model):
    __tablename__ = 'student'
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    age = db.Column(db.Integer)
    email = db.Column(db.String(120), unique=True, nullable=False)

    def __repr__(self):
        return f"<Student {self.name}>"

if __name__ == '__main__':
    # Create the database and tables
    db.create_all()
    app.run(debug=True)
```

### **Explanation:**
- **Association Table:** `teacher_student` is used to map the many-to-many relationship between `Teacher` and `Student`.
- **Teacher Model:** Contains attributes like `id`, `name`, `subject`, and `email`, and establishes a relationship with students.
- **Student Model:** Contains `id`, `name`, `age`, and `email`.
- The **`secondary`** keyword is used in the `Teacher` model to define the many-to-many relationship with students.
- The `backref` option allows students to access their teachers easily.

---

## **3. Running the Flask Application**

### **Steps to Run:**
1. **Install Dependencies:**
   Ensure you have Python and Flask installed. Then, install the required dependencies:
   ```bash
   pip install flask flask_sqlalchemy
   ```

2. **Run the Application:**
   Execute the following command to start the Flask application:
   ```bash
   python main.py
   ```

3. **Database:**
   The application will automatically create a `school.db` SQLite database in the project directory when you run it for the first time.

4. **Access the Application:**
   Open your browser and navigate to `http://127.0.0.1:5000/` to run the Flask app.

---

### **Conclusion:**
This project demonstrates:
- **Dynamic DOM Manipulation** using JavaScript to add and remove items from a list.
- **Flask-SQLAlchemy** usage to define models and establish a many-to-many relationship between **Teacher** and **Student**.

