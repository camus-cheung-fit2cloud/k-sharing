# Use the official Nginx image from the Docker Hub
FROM nginx:latest

# Replace the content of index.html with 'Hello' followed by the current timestamp
RUN echo "Hello - $(date)" > /usr/share/nginx/html/index.html