Index.html represents the first question dom significance and dynamc list manipulation with example
2. Flask-SQLAlchemy: Teacher-Student Many-to-Many Relationship
a. Diagram Illustrating the Relationship
The many-to-many relationship between Teacher and Student is implemented using an association table. The diagram below represents the relationship:
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
b. Flask-SQLAlchemy Script, code is in main.py
