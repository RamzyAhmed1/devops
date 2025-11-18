# --- Stage 1: Builder ---
FROM node:18-alpine as builder

WORKDIR /app
COPY package*.json ./

# Install only production dependencies
RUN npm install --omit=dev

# --- Stage 2: Final Image ---
FROM node:18-alpine

WORKDIR /app

# Create a non-root user for better security
RUN addgroup -S app && adduser -S app -G app

# Copy the built dependencies from the builder stage
COPY --from=builder /app/node_modules ./node_modules
COPY . .

# Switch to the non-root user
USER app

EXPOSE 80
CMD ["node", "server.js"]
