# Guide to Setting Up a Python Flask Backend and React Frontend on Vercel

This README provides detailed instructions for deploying a serverless Python Flask backend alongside a React frontend on Vercel.

## 1. Preparing Your Flask Backend

### Create Flask API

Organize your Flask application into a single file for simplicity or structure it appropriately for larger applications.

File: `/api/index.py`

```python
from flask import Flask, jsonify, request

app = Flask(__name__)

@app.route('/api/greet', methods=['GET'])
def greet():
    name = request.args.get('name', 'World')
    return jsonify({'message': f'Hello, {name}!'})

if __name__ == "__main__":
    app.run()
```

### Requirements File

Create a `requirements.txt` in the root directory:

```plaintext
Flask==2.0.1
gunicorn==20.1.0
```

### Configuration for Vercel

Vercel supports Python WSGI applications via a `vercel.json` configuration file.

File: `/vercel.json`

```json
{
  "version": 2,
  "builds": [{
    "src": "api/index.py",
    "use": "@vercel/python"
  }],
  "routes": [{
    "src": "/api/(.*)",
    "dest": "api/index.py"
  }]
}
```

## 2. Setting Up Your React Frontend

Create a new React application:

```bash
npx create-react-app my-app
cd my-app
```

### Sample React Component

Create a component that interacts with the Flask API.

File: `/src/App.js`

```javascript
import React, { useState } from 'react';

function App() {
  const [greeting, setGreeting] = useState('');

  const fetchGreeting = async () => {
    const response = await fetch('/api/greet?name=Vercel User');
    const data = await response.json();
    setGreeting(data.message);
  };

  return (
    <div className="App">
      <header className="App-header">
        <p>{greeting}</p>
        <button onClick={fetchGreeting}>Fetch Greeting</button>
      </header>
    </div>
  );
}

export default App;
```

## 3. Deploying on Vercel

- Push your project to a GitHub repository.
- Connect your GitHub repository to Vercel via the Vercel Dashboard.
- Set up the project, ensuring the build settings correctly point to the Flask and React applications.
- Deploy the changes.

## Conclusion

This setup allows you to run a Flask backend serverlessly with a React frontend on Vercel, offering a full-stack solution perfect for rapid development and scalable deployments.
