# Simple Frontend and Backend Application (Docker Compose)

This project demonstrates a basic full-stack application using Docker Compose. It consists of:
- A **Flask backend** that returns a simple JSON response.
- A **Frontend** served by Nginx displaying the message from the backend.

---

## Project Structure

```
Docker compose task/
│
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/
│   ├── index.html
│   └── Dockerfile
│
└── docker-compose.yml
```

---

## Backend

**File:** `backend/app.py`

```python
from flask import Flask, jsonify
app = Flask(__name__)

@app.route('/')
def home():
    return jsonify({"message": "Backend is working fine!"})

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

**Dependencies:**  
```
flask
```

**Dockerfile:**

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py ./
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## Frontend

**File:** `frontend/index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Simple Frontend</title>
</head>
<body>
    <h1>Simple Frontend</h1>
    <p id="message">Loading...</p>

    <script>
        fetch("http://localhost:5000/")
            .then(response => response.json())
            .then(data => {
                document.getElementById("message").innerText = data.message;
            })
            .catch(error => {
                document.getElementById("message").innerText = "Could not reach backend: " + error;
            });
    </script>
</body>
</html>
```

**Dockerfile:**

```dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
EXPOSE 80
```

---

## Docker Compose

**File:** `docker-compose.yml`

```yaml
services:
  backend:
    build: ./backend
    ports:
      - "5000:5000"

  frontend:
    build: ./frontend
    ports:
      - "8080:80"
    depends_on:
      - backend
```

---

## How to Run

1. Make sure Docker Desktop is running.
2. Open the terminal in the project root folder.
3. Run the following command:
   ```bash
   docker compose up --build
   ```
4. Visit the following URLs:
   - Frontend: [http://localhost:8080](http://localhost:8080)
   - Backend: [http://localhost:5000](http://localhost:5000)

---

## Output

When running successfully:
- Backend returns: `{ "message": "Backend is working fine!" }`
- Frontend displays: **Simple Frontend – Backend is working fine!**

---

## Screenshot

Add a screenshot of docker-compose running here before submitting your project.
