Step 1: Install Flask

Open your terminal or command prompt.
Run the following command to install Flask:
Copy code
pip install Flask
Step 2: Create Database

In the same directory where your Python scripts are located, create a new file named models.py.
Open models.py and copy the following code into it:
python
Copy code
# models.py

import sqlite3

conn = sqlite3.connect('assistants.db')
c = conn.cursor()

# Create assistants table
c.execute('''CREATE TABLE IF NOT EXISTS assistants
             (id INTEGER PRIMARY KEY AUTOINCREMENT,
              name TEXT NOT NULL,
              role TEXT NOT NULL,
              email TEXT UNIQUE NOT NULL,
              phone_number TEXT UNIQUE NOT NULL,
              created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP)''')

conn.commit()
conn.close()
Save the file.
Step 3: Run Database Creation Script

In your terminal, navigate to the directory containing models.py.
Run the following command to execute the script and create the SQLite database:
Copy code
python models.py
Step 4: Run the Flask Application

Create another new file in the same directory named app.py.
Open app.py and copy the following code into it:
python
Copy code
# app.py

from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

# Get all assistants
@app.route('/assistants', methods=['GET'])
def get_assistants():
    conn = sqlite3.connect('assistants.db')
    c = conn.cursor()
    c.execute("SELECT * FROM assistants")
    assistants = c.fetchall()
    conn.close()
    return jsonify(assistants)

# Create an assistant
@app.route('/assistants', methods=['POST'])
def create_assistant():
    data = request.get_json()
    conn = sqlite3.connect('assistants.db')
    c = conn.cursor()
    c.execute("INSERT INTO assistants (name, role, email, phone_number) VALUES (?, ?, ?, ?)",
              (data['name'], data['role'], data['email'], data['phone_number']))
    conn.commit()
    conn.close()
    return jsonify({"message": "Assistant created successfully"})

# Update an assistant
@app.route('/assistants/<int:id>', methods=['PATCH'])
def update_assistant(id):
    data = request.get_json()
    conn = sqlite3.connect('assistants.db')
    c = conn.cursor()
    c.execute("UPDATE assistants SET name=?, role=?, email=?, phone_number=? WHERE id=?",
              (data['name'], data['role'], data['email'], data['phone_number'], id))
    conn.commit()
    conn.close()
    return jsonify({"message": "Assistant updated successfully"})

# Delete an assistant
@app.route('/assistants/<int:id>', methods=['DELETE'])
def delete_assistant(id):
    conn = sqlite3.connect('assistants.db')
    c = conn.cursor()
    c.execute("DELETE FROM assistants WHERE id=?", (id,))
    conn.commit()
    conn.close()
    return jsonify({"message": "Assistant deleted successfully"})

if __name__ == '__main__':
    app.run(debug=True)
Save the file.
Step 5: Run the Flask Application

In your terminal, navigate to the directory containing app.py.
Run the following command to start the Flask server:
Copy code
python app.py
You should see output similar to Running on http://127.0.0.1:5000/.
Step 6: Test the APIs

Use an API testing tool like Postman.
Send requests to the following endpoints to interact with the CRUD APIs:
GET http://127.0.0.1:5000/assistants to retrieve all assistants.
POST http://127.0.0.1:5000/assistants with JSON data to create a new assistant.
PATCH http://127.0.0.1:5000/assistants/<id> with JSON data to update an existing assistant.
DELETE http://127.0.0.1:5000/assistants/<id> to delete an existing assistant.
That's it! You have successfully set up and run the backend application with CRUD APIs for managing assistants using Flask and SQLite database. You can now test the APIs using Postman or any other HTTP client.

User
it give  error Traceback (most recent call last):
  File "c:\Users\asus\app.py", line 3, in <module>
    from flask import Flask, request, jsonify
ModuleNotFoundError: No module named 'flask'
ChatGPT
It seems that Flask is not installed in your Python environment. Here's how you can install Flask:

Step 1: Install Flask

Open your terminal or command prompt.
Run the following command to install Flask:
pip install Flask
Step 2: Verify Flask Installation

After installing Flask, you can verify if it's installed correctly by running:
sql
pip show Flask
This command should display information about the Flask package, including the version number.
Step 3: Run the Application

Once Flask is installed, you can try running the application again:
python app.py
If Flask is installed correctly, the application should start without any errors.
