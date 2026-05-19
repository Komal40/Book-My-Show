# Stage 1: Build stage
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files
COPY package.json package-lock.json ./

# Install dependencies with legacy peer deps flag
RUN npm install --legacy-peer-deps

# Copy source code
COPY . .

# Build the React app
RUN npm run build

# Stage 2: Production stage
FROM node:18-alpine

WORKDIR /app

# Install serve to run the production build
RUN npm install -g serve

# Copy built application from builder stage
COPY --from=builder /app/build ./build
COPY --from=builder /app/package.json ./

# Expose port 3000
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Start the application
CMD ["serve", "-s", "build", "-l", "3000"]


# # Use Node.js 18 (or your Jenkins-configured version)
# FROM node:18

# # Set working directory
# WORKDIR /app

# # Copy package.json and package-lock.json
# COPY package.json package-lock.json ./

# # Force install a compatible PostCSS version to fix the issue
# RUN npm install postcss@8.4.21 postcss-safe-parser@6.0.0 --legacy-peer-deps

# # Install dependencies
# RUN npm install

# # Copy the entire project
# COPY . .

# # Expose port 3000
# EXPOSE 3000

# # Set environment variable to prevent OpenSSL errors
# ENV NODE_OPTIONS=--openssl-legacy-provider
# ENV PORT=3000

# # Start the application
# CMD ["npm", "start"]
