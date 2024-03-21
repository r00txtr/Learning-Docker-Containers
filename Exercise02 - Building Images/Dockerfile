# Use the official Nginx image as the base image
FROM nginx

# Copy custom configuration file from the current directory to Nginx's configuration directory
COPY nginx.conf /etc/nginx/nginx.conf

# Copy index.html file to Nginx's default HTML directory
COPY index.html /usr/share/nginx/html/

# Expose port 80
EXPOSE 80
