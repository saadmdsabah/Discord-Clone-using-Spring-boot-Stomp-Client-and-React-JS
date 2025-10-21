# Discord Clone 🚀

A real-time, full-stack chat application inspired by Discord. This project leverages a powerful combination of Spring Boot for the backend, React JS for the frontend, and WebSockets for seamless, bidirectional communication. The entire application is containerized using Docker for easy setup and deployment.

<img width="1401" height="412" alt="image" src="https://github.com/user-attachments/assets/5dd27a47-f7c4-4bde-91ba-fc1c3ca9a932" />

## 🏛️ Architecture Overview

The application is designed with a decoupled frontend and backend architecture:

* **Frontend**: A responsive user interface built with **React JS**. It is served statically using an **Nginx** web server.
* **Backend**: A robust API built with **Spring Boot**. It handles business logic, user authentication, and message persistence.
* **Real-time Communication**: **STOMP over WebSocket** is used to enable real-time messaging between the clients and the server.
* **Database**: **MongoDB** is used as the NoSQL database to store messages, user data.
* **Containerization**: The entire stack (Frontend, Backend, Database) is orchestrated using **Docker Compose**, ensuring a consistent development and production environment.

---

## 💻 Tech Stack

| Area      | Technology                                                                                                                                                                             |
| :-------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backend** | ![Java](https://img.shields.io/badge/Java-21-blue) ![Spring](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen) ![WebSocket](https://img.shields.io/badge/WebSocket-STOMP-orange) |
| **Frontend**| ![React](https://img.shields.io/badge/React-18-blue) ![Vite](https://img.shields.io/badge/Vite-JS-yellow) ![StompJS](https://img.shields.io/badge/Stomp.js-client-red)                    |
| **Database**| ![MongoDB](https://img.shields.io/badge/MongoDB-blue)                                                                                                                                  |
| **DevOps** | ![Docker](https://img.shields.io/badge/Docker-blue) ![Nginx](https://img.shields.io/badge/Nginx-green)                                                                                 |

---

## 📂 Project Structure

This repository acts as a parent project that orchestrates the backend and frontend using Docker. The actual source code for the frontend and backend services resides in separate Git submodules.

* `chat-application-backend/`: The Spring Boot backend service.
* `chat-application-frontend/`: The React JS frontend application.
* `docker-compose.yml`: The Docker Compose file to build and run the entire application stack.

---

## 🚀 Getting Started

You can get the entire application running on your local machine with just a few commands, thanks to Docker.

### Prerequisites

* [Git](https://git-scm.com/)
* [Docker](https://www.docker.com/products/docker-desktop/)
* [Docker Compose](https://docs.docker.com/compose/install/)
### Method 1: Build from Source

1.  **Clone the repository with its submodules:**
    ```bash
    git clone --recurse-submodules https://github.com/saadmdsabah/Discord-Clone-using-Spring-boot-Stomp-Client-and-React-JS.git
    ```

2.  **Navigate into the project directory:**
    ```bash
    cd Discord-Clone-using-Spring-boot-Stomp-Client-and-React-JS
    ```

3.  **Build and run the application using Docker Compose:**
    ```bash
    docker-compose up --build
    ```
    This will build the custom Docker images from the source code and then start all the services.

### Method 2: Use Pre-built Images (Faster)

If you don't need to build the images from the source code, you can pull them directly from Docker Hub.

1.  **Pull the latest pre-built images:**
    ```bash
    docker pull saadsabahuddin/discord-clone-backend:latest
    docker pull saadsabahuddin/discord-clone-frontend:latest
    ```

2.  **Clone the repository (submodules are not needed):**
    ```bash
    git clone [https://github.com/saadmdsabah/Discord-Clone-using-Spring-boot-Stomp-Client-and-React-JS.git](https://github.com/saadmdsabah/Discord-Clone-using-Spring-boot-Stomp-Client-and-React-JS.git)
    cd Discord-Clone-using-Spring-boot-Stomp-Client-and-React-JS
    ```

3.  **Start the application:**
    ```bash
    docker-compose up
    ```
    This will use the images you pulled instead of building them locally.

### Access the Application

Once the containers are running (using either method), open your web browser and navigate to:
**[http://localhost:5173](http://localhost:5173)**

---

## Submodule Details

For more detailed information about the frontend or backend implementation, please visit their respective repositories.

  * Backend Repository: **[saadmdsabah/chat-application-backend](https://github.com/saadmdsabah/chat-application-backend/tree/281bd5ea6ce9bb99b2dcd4e8344642e3bc453b68)**

  * Frontend Repository: **[saadmdsabah/chat-application-frontend](https://github.com/saadmdsabah/chat-application-frontend/tree/47bbe350c7a3f3db73b68dea057fdb28f53f7f3f)**

## 📜 License

This project is licensed under the MIT License. See the LICENSE file for more details.


