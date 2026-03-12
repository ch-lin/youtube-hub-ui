# YouTube Hub UI (Frontend)

![Next.js](https://img.shields.io/badge/Next.js-16-black)
![React](https://img.shields.io/badge/React-19-blue)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED)
![License](https://img.shields.io/badge/License-MIT-green)

The **YouTube Hub UI** is the modern, responsive web interface for the **YouTube Data Hub** platform.

Built with **Next.js 16**, it provides a user-friendly dashboard to manage channel subscriptions, browse video assets, and trigger download tasks without touching the command line. It acts as the visual facade, communicating with the Core Backend and Authentication Service.

## 🏗️ Architecture & Role

*   **The "Face"**: Delivers the visual interface for the system.
*   **Hybrid Rendering**: Utilizes Next.js App Router for optimal performance, mixing Server Components for data fetching and Client Components for interactivity.
*   **API Consumer**: Aggregates data from the **YouTube Hub Service** (Core) and handles user sessions via the **Authentication Service**.
*   **Secure Proxy**: Uses Next.js API Routes (or Server Actions) to proxy requests to backend services, keeping internal API URLs hidden and managing CORS effectively.

## 🚀 Key Features

*   **Channel Dashboard**: Visual management of tracked YouTube channels.
*   **Asset Library**: Browse videos with rich metadata (thumbnails, duration, publish date).
*   **Real-time Status**: Visual indicators for download status (Pending, Downloading, Completed, Failed).
*   **Download Manager**: GUI for configuring `yt-dlp` options (Quality, Format, Audio-only) before triggering a download.
*   **Authentication**: Seamless login integration using JWTs issued by the backend Auth Service.

## 🛠️ Tech Stack

*   **Framework**: Next.js 16 (App Router)
*   **Library**: React 19
*   **Styling**: Tailwind CSS
*   **Package Manager**: pnpm
*   **Containerization**: Docker (Multi-stage build)

## 📦 Prerequisites

To run this frontend, you need the backend services running:

1.  **Authentication Service**: For logging in.
2.  **YouTube Hub Service**: For fetching channel/video data.
3.  **Node.js 22+**: If running locally without Docker.

## ⚙️ Configuration

Create a `.env` file in the root directory. You can copy `.env.example` if available.

### Runtime Environment Variables
These are used by the Next.js server (Node.js environment) at runtime.

```properties
# Internal URL for Next.js server to access the Backend API (Server-side)
PUBLIC_BACKEND_URL=http://localhost:8080

# Internal URL for Next.js server to access the Auth Service (Server-side)
AUTH_SERVICE_API_URL=http://localhost:8081

# NextAuth Configuration
AUTH_SECRET=your-generated-secret-key
# NEXTAUTH_URL=http://localhost:3000 # Optional in some setups
```

### Build-Time Variables
These are baked into the JavaScript bundle for the browser during the build process.

```properties
# Public URL for the browser to access the Backend API (Client-side)
NEXT_PUBLIC_BROWSER_API_URL=http://localhost:8080
```

## 🏃‍♂️ Build & Run

### Local Development

```bash
# 1. Install dependencies
cd youtube-hub-ui
pnpm install

# 2. Run development server
pnpm dev
```
Access the UI at `http://localhost:3000`.

### Docker Production Build

The Dockerfile uses a multi-stage build to create a lightweight standalone image.

```bash
# 1. Build the image
# Note: You must pass the build argument for the public API URL
docker build \
  --build-arg NEXT_PUBLIC_BROWSER_API_URL=http://localhost:8080 \
  -t youtube-data-hub/ui-service .
```

```bash
# 2. Run the container
docker run -d \
  -p 3000:3000 \
  --env-file .env \
  youtube-data-hub/ui-service
```

## 🔐 Authentication Integration

The UI uses **NextAuth.js** to handle user sessions.
1.  **Login**: User enters credentials.
2.  **Verification**: Next.js server calls the **Authentication Service** to verify credentials.
3.  **Session**: On success, a JWT is stored in an HTTP-only cookie.
4.  **API Calls**: Subsequent requests to the Backend API include this JWT in the `Authorization` header.

## 📜 License

MIT
