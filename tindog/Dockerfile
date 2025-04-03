# Use the official Nginx image as base
FROM nginx:latest

# Remove default index page
RUN rm -rf /usr/share/nginx/html/*

# Copy your project files to the Nginx web root
COPY . /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
