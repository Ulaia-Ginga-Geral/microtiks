FROM node:20-alpine

# Set working directory
WORKDIR /app

# Install system dependencies and NestJS CLI
RUN apk add --no-cache python3 make g++
RUN npm install -g @nestjs/cli

# Copy package files
COPY package*.json ./

# Install dependencies with robust network retry configuration
RUN npm config set registry http://registry.npmjs.org/ && \
    npm config set maxsockets 3 && \
    npm config set fetch-retry-mintimeout 20000 && \
    npm config set fetch-retry-maxtimeout 120000 && \
    npm config set fetch-retries 10 && \
    npm install --no-audit --no-fund

# Copy source code
COPY . .

# Build the application (skip if fails)
RUN npm run build || echo "Build failed, using source"

# Create non-root user
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nestjs -u 1001

# Change ownership of app directory
RUN chown -R nestjs:nodejs /app
USER nestjs

# Expose port
EXPOSE 4000

# Start the application
CMD ["npm", "run", "start:dev"]
