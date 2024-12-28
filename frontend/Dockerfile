# Stage 1: Build the React app
FROM node:16-alpine AS build

WORKDIR /app

# Copy the package.json file and install dependencies
COPY package.json ./
RUN npm install

# Copy the rest of the application and build the React app
COPY . ./
RUN npm run build

# Stage 2: Set up Nginx to serve the React app
FROM nginx:alpine AS nginx

# Copy the build output from the build stage into the Nginx container
COPY --from=build /app/build /usr/share/nginx/html

# Expose port 80 for Nginx
EXPOSE 80
