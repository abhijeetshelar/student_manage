# Use node:16 as the base image
FROM node:16

# Set the working directory in the container
WORKDIR /app

# Add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH

# Install nginx and clean up unnecessary files to reduce image size
RUN apt-get update && apt-get install -y nginx && \
    rm -rf /var/lib/apt/lists/*

# Copy custom nginx configuration
COPY nginx.conf /etc/nginx/sites-available/default

# Append 'daemon off' to nginx.conf to keep the process running in the foreground
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf

# Copy application files into the container
COPY ./ /app/

# Install app dependencies, build the app, and copy build output to nginx's web folder
RUN yarn install && yarn build && \
    cp -R build/* /var/www/html/

# Forward nginx logs to Docker's standard output and error logs
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log

# Expose port 80 for nginx
EXPOSE 80

# Start nginx to serve the app
CMD ["nginx"]
