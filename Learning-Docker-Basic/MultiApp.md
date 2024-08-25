---

# Multi-Container Apps

## Overview
Integrate MySQL with your application by using separate containers for each service. This approach allows for better scaling, isolation, and flexibility. Here's how to set up a MySQL container alongside your app.

## Container Networking
Containers run in isolation. To enable communication between containers, they must be on the same network.

### Create Network
```bash
docker network create todo-app
```

### Start MySQL Container
```bash
docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:8.0
```

**Note:** Docker creates the named volume `todo-mysql-data` automatically.

### Verify MySQL
```bash
docker exec -it <mysql-container-id> mysql -u root -p
```
Password: `secret`

Verify the database:
```sql
SHOW DATABASES;
```

## Connect to MySQL
Use the `nicolaka/netshoot` container for DNS resolution and troubleshooting:
```bash
docker run -it --network todo-app nicolaka/netshoot
```
Find MySQL container IP:
```bash
dig mysql
```

## Run Your App
Start your app container and connect it to the MySQL database using environment variables:
Note: Make sure that you are in the getting-started-app directory when you run this command.
```bash
docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```

Check logs to confirm database connection:
```bash
docker logs -f <container-id>
```

## Verify Data Storage
Connect to MySQL and check stored items:
```bash
docker exec -it <mysql-container-id> mysql -p todos
```
Run SQL query:
```sql
SELECT * FROM todo_items;
```

---
