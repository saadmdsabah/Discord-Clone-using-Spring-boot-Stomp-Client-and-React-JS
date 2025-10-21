# Discord Clone 🚀

A real-time, full-stack chat application inspired by Discord. This project leverages a powerful combination of Spring Boot for the backend, React JS for the frontend, and WebSockets for seamless, bidirectional communication. The entire application is containerized using Docker for easy setup and deployment.

<img width="1401" height="412" alt="image" src="https://github.com/user-attachments/assets/5dd27a47-f7c4-4bde-91ba-fc1c3ca9a932" />

## 🏛️ Architecture Overview

The application is built on a modern, decoupled architecture designed for real-time performance and scalability. Each component is containerized to ensure a consistent and portable environment.

* **Frontend**: A dynamic user interface built with **React** and bundled by **Vite**. Global state is managed efficiently by **Redux Toolkit**, with user sessions seamlessly maintained across reloads using **Redux Persist**. The responsive design is crafted with **Tailwind CSS**, and the final static build is served by an **Nginx** web server.

* **Backend**: A robust and secure RESTful API powered by **Spring Boot**. It handles all business logic, while **Spring Security** protects the endpoints and manages user authentication and authorization using **JWT** (JSON Web Tokens).

* **Real-time Communication**: Instant, bidirectional messaging is powered by **STOMP over WebSocket**. The Spring backend provides the message broker, and the React frontend connects using **STOMP.js**. This enables live chat and delivers instant UI updates and notifications via **React Hot Toast**.

* **Database**: **MongoDB** database acts as the primary data store, persisting all user information, group details, and message history.

* **Containerization**: The entire ecosystem—frontend, backend, and database—is orchestrated by **Docker Compose**. This approach guarantees a simple, one-command setup and a consistent environment from development to production.

---

## 💻 Tech Stack

| Area | Technology |
| :--- | :--- |
| **Backend** | ![Java](https://img.shields.io/badge/Java-21-blue) ![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen) ![WebSocket](https://img.shields.io/badge/WebSocket-STOMP-orange) ![JWT](https://img.shields.io/badge/JWT-Authentication-black) |
| **Frontend**| ![React](https://img.shields.io/badge/React-18-blue) ![Vite](https://img.shields.io/badge/Vite-JS-yellow) ![Redux](https://img.shields.io/badge/Redux-Toolkit-764ABC) ![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC) ![StompJS](https://img.shields.io/badge/Stomp.js-client-red) ![Hot Toast](https://img.shields.io/badge/React_Hot_Toast-FF4154) |
| **Database**| ![MongoDB](https://img.shields.io/badge/MongoDB-blue) |
| **DevOps** | ![Docker](https://img.shields.io/badge/Docker-blue) ![Nginx](https://img.shields.io/badge/Nginx-green) |

---

## ⚙️ Application Workflow

The following diagrams illustrate the core operational flows of the application.

## User Authentication & Session Management

<img width="1402" height="804" alt="Screenshot 2025-10-21 135510" src="https://github.com/user-attachments/assets/bbe6c706-1dd0-4de5-86fa-096d6bdbf9e3" />

The authentication process is initiated when a user submits their credentials. The **Spring Boot** backend validates these against the **MongoDB** database. Upon success, a **JWT** is generated and returned to the client. The **React** frontend then persists the token and user data in the **Redux Store** and establishes a WebSocket connection for real-time updates.

## Group Management & Join Requests

<img width="1237" height="755" alt="Screenshot 2025-10-21 140217" src="https://github.com/user-attachments/assets/52df9c0d-0404-44c3-944a-f93f954239f1" />

Users can create public or private groups. Joining a public group is instantaneous, with the backend directly adding the user. For private groups, a join request is sent to the backend, which then notifies the group owner in real-time. The owner's decision (accept/reject) is processed by the backend, updating the request status and notifying the original user of the outcome via WebSocket.

## Real-Time Messaging

<img width="1797" height="551" alt="Screenshot 2025-10-21 140334" src="https://github.com/user-attachments/assets/45a12f35-cca4-41e6-a06c-7b2f89566a9f" />

Once connected, a user sends a message through the **WebSocket Client**. The message is transmitted to the **Spring Boot** backend via its STOMP endpoint. The backend first persists the message in **MongoDB** for chat history and then broadcasts it to all members subscribed to that specific group's topic, ensuring instantaneous delivery.

## User Logout

<img width="1339" height="242" alt="Screenshot 2025-10-21 140035" src="https://github.com/user-attachments/assets/d32668ec-b0f9-4fe0-853e-5f6af208bc60" />

During logout, the client-side action clears the user's session data from the **Redux Store** and removes the persisted state. Simultaneously, it sends a command to disconnect from the **WebSocket Server**, gracefully terminating the real-time communication channel.

---

## ⚡ Caching Layer (Redis)

To enhance performance and reduce database load, the application integrates **Redis** as a distributed caching layer. Frequently accessed data that doesn't change often is cached in-memory, leading to significantly faster API responses for repeat requests.

The caching logic is implemented in the Spring Boot backend using Spring's Cache Abstraction. When data is modified (e.g., a user joins a new room or an invitation is accepted), the relevant caches are automatically **evicted** (cleared) to prevent stale data and ensure consistency across the application.

The following data is cached:

| Cache Name            | Cached Data         | Description                                | Relevant API Endpoints                         |
| :-------------------- | :------------------ | :----------------------------------------- | :------------------------------------------- |
| `joinedRooms`         | List of rooms       | A user's list of joined chat rooms.        | `GET /api/v1/room/joined/{userName}`         |
| `createdRooms`        | List of rooms       | A user's list of created chat rooms.       | `GET /api/v1/room/created/{userName}`        |
| `invitationsBySender` | List of invitations | A user's pending sent invitations.         | `GET /api/v1/invitation/sent/{userName}`     |
| `invitationsByReceiver`| List of invitations | A user's pending received invitations.     | `GET /api/v1/invitation/received/{userName}` |
| `checkIfEntryPossible`| Boolean             | Verifies if a user is a member of a room.  | `GET /api/v1/room/is-joined/{roomId}/{userName}`|
| `roomCreator`         | String (username)   | The creator of a specific room.            | `GET /api/v1/room/creator/{roomId}`          |

---

## 🔌 WebSocket API Endpoints

The application uses STOMP over WebSocket for real-time communication. Clients can send messages to the destinations listed below to perform actions, and they should subscribe to the broadcast topics to receive real-time updates.

| Endpoint | Description | Payload | Broadcast Topic(s) |
| :--- | :--- | :--- | :--- |
| `/sendMessage/{roomId}` | Sends a chat message to a specific room. The returned message is broadcasted to all members. | `MessageDto` | `/topic/rooms/{roomId}` |
| `/createRoom/{roomId}` | Creates a new public or private chat room. | `CreateRoomDto` | `/topic/rooms/{userName}` (Notifies the creator to join the new room) |
| `/addRoom/{userName}/{roomId}` | Adds a user to an existing public room. | None | `/topic/rooms/{userName}` (Notifies the user to join the new room) |
| `/createInvitation` | Sends an invitation to a user to join a private room. | `CreateInvitationDto` | `/topic/sentInvitations/{sender}` <br> `/topic/receivedInvitations/{receiver}` |
| `/updateStatusOfInvitation` | Updates an invitation's status to `ACCEPTED` or `REJECTED`. | `UpdateInvitationStatusDto` | **If Accepted:** <br> `/topic/remove/sentInvitations/{sender}` <br> `/topic/rooms/{sender}` <br><br> **If Rejected:** <br> `/topic/remove/sentInvitations/{sender}` |

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

  * Backend Repository: **[saadmdsabah/chat-application-backend](https://github.com/saadmdsabah/chat-application-backend/tree/a6dc5b7f7631b29113003c9eb6a659a1896b0bad)**

  * Frontend Repository: **[saadmdsabah/chat-application-frontend](https://github.com/saadmdsabah/chat-application-frontend/tree/47bbe350c7a3f3db73b68dea057fdb28f53f7f3f)**

## 📜 License

This project is licensed under the MIT License. See the LICENSE file for more details.






