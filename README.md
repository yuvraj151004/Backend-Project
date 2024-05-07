# Backend-Project

from flask import Flask, request, jsonify
import sqlite3

app = Flask(__name__)

# Function to establish database connection
def get_db_connection():
    conn = sqlite3.connect('assistants.db')
    conn.row_factory = sqlite3.Row
    return conn

# API endpoint to create a new assistant
@app.route('/assistants', methods=['POST'])
def create_assistant():
    # Parse JSON data from request body
    assistant_data = request.json
    name = assistant_data['name']
    role = assistant_data['role']
    email = assistant_data['email']
    phone_number = assistant_data['phone_number']

    # Insert assistant data into database
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('''
        INSERT INTO assistants (name, role, email, phone_number) 
        VALUES (?, ?, ?, ?)
    ''', (name, role, email, phone_number))
    conn.commit()

    # Get the ID of the last inserted assistant
    assistant_id = cursor.lastrowid

    # Close database connection
    conn.close()

    # Return a JSON response with the ID of the created assistant
    return jsonify({'id': assistant_id}), 201

if __name__ == '__main__':
    app.run(debug=True)
