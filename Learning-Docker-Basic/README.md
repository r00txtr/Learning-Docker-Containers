### 1. Installing Docker
First, you need to install Docker on your Virtual Machine (VM). Here’s how you can do it on different platforms:

- **Ubuntu/Linux**:
  1. Update your package index:
     ```bash
     sudo apt-get update
     ```
  2. Install the required packages:
     ```bash
     sudo apt-get install \
     ca-certificates \
     curl \
     gnupg \
     lsb-release
     ```
  3. Add Docker’s official GPG key:
     ```bash
     sudo apt-get update
     sudo apt-get install ca-certificates curl
     sudo install -m 0755 -d /etc/apt/keyrings
     sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
     sudo chmod a+r /etc/apt/keyrings/docker.asc
     ```
  4. Set up the stable repository:
     ```bash
     echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     ```
  5. Install Docker Engine:
     ```bash
     sudo apt-get update
     sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
     ```
  6. For permission problem:
     ```bash
     sudo chown $USER /var/run/docker.sock
     sudo usermod -a -G docker $USER
     sudo systemctl restart docker
     ```
  7. Verify that Docker is installed correctly by running:
     ```bash
     sudo docker run hello-world
     ```  7ca. Verify that Docker is installed correctly by running:
     ```bash
     sudo docker run hello-world
     ```

- **Windows/Mac**:
  1. Download Docker Desktop from the official [Docker website](https://www.docker.com/products/docker-desktop).
  2. Follow the installation instructions provided for your operating system.
  3. Once installed, verify it by running:
     ```bash
     docker run hello-world
     ```

### 2. Creating Dockerfiles
A Dockerfile is a script containing a series of commands to build a Docker image. Here’s a basic example:

1. **Create a new file named `Dockerfile`** in your project directory.
2. **Add the following contents** to your Dockerfile:

   ```dockerfile
   # Use an official Python runtime as a parent image
   FROM node:18-alpine

   # Set the working directory in the container
   WORKDIR /app

   # Copy the current directory contents into the container at /app
   COPY /getting-started-app/. .

   # Install any needed packages specified
   RUN yarn install --production

   # Make port 80 available to the world outside this container
   EXPOSE 3000

   # Run app.py when the container launches
   CMD ["node", "src/index.js"]
   ```

3. **Explanation**:
   - `FROM` sets the base image.
   - `WORKDIR` sets the working directory inside the container.
   - `COPY` copies files from your host machine to the container.
   - `RUN` executes commands (like installing dependencies).
   - `EXPOSE` makes a port available.
   - `CMD` defines the command to run when the container starts.

### 3. Building and Running Containers
Once you have your Dockerfile, you can build and run your Docker container:

1. **Build the Docker image**:
   ```bash
   docker build -t my-new-app .
   ```
   This command builds the Docker image from your Dockerfile. `-t my-python-app` tags the image with a name.

2. **Run the Docker container**:
   ```bash
   docker run -p 3000:80 my-new-app
   ```
   This command runs the container, mapping port 80 in the container to port 4000 on your host machine.

3. **Access your running application** by navigating to `http://localhost:4000` in your browser.

4. **Run the Docker container with Persist the todo data app**:
   ```bash
   docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
   ```
5. **Run the Docker container with bind mounts**:
   ```bash
   docker run -it --mount type=bind,src="$(pwd)",target=/src ubuntu:alpine bash
   ```
6. **Run the following command from the getting-started-app directory**:
   ```bash
   docker run -dp 0.0.0.0:3000:3000 -w /app --mount type=bind,src="$(pwd)",target=/app  node:18-alpine sh -c "yarn install && yarn run dev"
   ```
### 4. Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. Here's how to get started:

1. **Create a `docker-compose.yml` file** in your project directory:

   ```yaml
   services:
  app:
    image: node:18-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 0.0.0.0:3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:8.0
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos

volumes:
  todo-mysql-data:
   ```

2. **Explanation**:
   - `version` specifies the version of Docker Compose.
   - `services` defines the services in your application.
   - `web` is your web application, built from the Dockerfile in the current directory.
   - `redis` is an additional service that your web application might depend on (e.g., for caching).

3. **Run your multi-container application**:
   ```bash
   docker-compose up
   ```
   This command builds, (re)creates, starts, and attaches to containers for a service.

4. **Stop the application**:
   ```bash
   docker-compose down
   ```
