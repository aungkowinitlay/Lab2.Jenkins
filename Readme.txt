Checkout (on VM1): Clones the repository from GitHub.
Test VM2 Connection (on VM2): Verifies connectivity to VM2.
Build Backend Image (on VM1): Builds the aungkowin/flask-backend:latest Docker image.
Build Frontend Image (on VM1): Builds the aungkowin/nginx-frontend:latest Docker image.
Test Backend (on VM1): Placeholder for backend tests (currently just an echo statement).
Login to Docker Hub (on VM1): Logs into Docker Hub using credentials.
Push Images (on VM1): Pushes the backend and frontend images to Docker Hub.
Deploy Backend (on VM3): Deploys the Flask backend using docker-compose.
Deploy Frontend (on VM2): Deploys the Nginx frontend using docker-compose.
Post Actions (on VM1): Logs out of Docker Hub and prints success/failure messages.