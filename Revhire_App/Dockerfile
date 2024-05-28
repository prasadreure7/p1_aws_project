
# Set the Node.js version to use
ARG NODE_VERSION=22.1.0

################################################################################
# Stage 1: Base
FROM node:${NODE_VERSION}-alpine as base

# Set the working directory
WORKDIR /usr/src/app

################################################################################
# Stage 2: Dependencies
FROM base as deps

# Copy package.json and package-lock.json for dependency resolution
COPY package.json package-lock.json ./

# Install all dependencies (both production and development)
RUN npm install

################################################################################
# Stage 3: Build
FROM deps as build

# Copy the rest of the source files
COPY . .

# Install Angular CLI globally
RUN npm install -g @angular/cli@16.2.12

# Build the Angular application
RUN npm run build

################################################################################
# Stage 4: Final
FROM base as final

# Set the node environment to production
ENV NODE_ENV production

# Install Angular CLI globally in the final stage
RUN npm install -g @angular/cli@16.2.12

# Copy the entire project from the build stage
COPY . .

# Copy the node_modules from the deps stage
COPY --from=deps /usr/src/app/node_modules ./node_modules

# Expose the port that the application runs on
EXPOSE 4200

# Run the application using Angular CLI
CMD ["ng", "serve", "--host", "0.0.0.0"]
