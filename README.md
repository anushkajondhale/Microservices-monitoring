# 🛠️ Microservices Project with Flask, PostgreSQL, Redis, and Docker Compose

This project demonstrates a **microservices-based architecture** using Python Flask APIs with inter-service communication, caching, and container orchestration.

---

## 📦 Services

- **user-service** – Handles user registration, listing, and deletion using PostgreSQL  
- **data-service** – Fetches users by name from user-service, caches results with Redis  

Built with Docker Compose, it showcases real-world patterns like service discovery, caching, healthchecks, and persistent storage.

---

## 🔧 Tech Stack

| Layer            | Technology               |
|------------------|--------------------------|
| API Framework    | Flask (Python)           |
| Database         | PostgreSQL               |
| Caching Layer    | Redis                    |
| Containerization | Docker, Docker Compose   |
| API Communication| REST (requests module)   |
| Service Discovery| Docker Network           |

---

## ⚙️ Architecture

          ┌─────────────────────┐
          │   data-service      │
          │  (Flask + Redis)    │
          └────────┬────────────┘
                   │
                   ▼
          ┌─────────────────────┐
          │   user-service      │
          │  (Flask + PostgreSQL)│
          └────────┬────────────┘
                   │
                   ▼
           ┌──────────────┐
           │  PostgreSQL  │
           └──────────────┘


- **Redis** handles caching of `/user/<name>` lookups to reduce DB/API calls.

---

## 🚀 How to Run the Project

1. **Clone the Repository**
   ```bash
   git clone https://github.com/abhi-gadge1773/your-repo-name.git
   cd your-repo-name
Build and Start Services

docker compose up --build


All services (user-service, data-service, postgres, redis) will spin up on a shared Docker network.

📂 Project Structure
.
├── docker-compose.yml
├── init.sql
├── user-service/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app.py
├── data-service/
│   ├── Dockerfile
│   ├── requirements.txt
│   └── app.py

🧪 API Endpoints
📘 user-service – localhost:5000
Method	Endpoint	Description
POST	/register	Register a new user
GET	/users	Get all users
DELETE	/delete/<name>	Delete a user by name

Example:

curl -X POST http://localhost:5000/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Abhijeet", "email":"abhi@example.com"}'

📗 data-service – localhost:5001
Method	Endpoint	Description
GET	/user/<name>	Get user info (uses Redis cache)

Example:

curl http://localhost:5001/user/Abhijeet


First call → cached: false

Second call → cached: true (from Redis)

🔁 Redis Caching Logic

Request /user/<name>

Service first checks Redis for cached data

If not found → calls user-service to fetch users

Stores result in Redis for future use

Next request is served instantly from Redis

🗃️ PostgreSQL Setup

The schema is initialized using init.sql:

CREATE TABLE IF NOT EXISTS users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL
);

🧼 Clean Up

To stop and remove all containers and volumes:

docker compose down -v

🤝 Contribution

Feel free to fork, improve, and submit pull requests. Suggestions are always welcome!

📄 License

This project is licensed under the MIT License.
