# Start with the official Node.js Docker image
FROM node:alpine

# Set the working directory in the Docker image
WORKDIR /usr/src/app

# Copy package.json and package-lock.json (if available)
COPY package*.json ./

# Install the application dependencies
RUN npm install

# Copy all other source code to work directory
COPY . /usr/src/app

# Set the application's default port
EXPOSE 5001

# Run the application
CMD [ "npm", "start" ]

