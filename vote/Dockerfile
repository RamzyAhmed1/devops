# --- Stage 1: Builder ---
# Note: switched to the full image (removed the -slim variant)
FROM python:3.10-bullseye as builder

WORKDIR /app

# Create a non-root user for better security
RUN addgroup --system app && adduser --system --group app

# Copy dependency file first to optimize Docker caching
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# --- Stage 2: Final Image ---
# Using the full base image as well (no -slim)
FROM python:3.10-bullseye

WORKDIR /app

# Bring over the user setup and installed dependencies from the builder stage
COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /etc/group /etc/group
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy the application source code
COPY . .

# Switch to the non-root user
USER app

# Start the Flask application
CMD ["flask", "run", "--host=0.0.0.0", "--port=80"]
