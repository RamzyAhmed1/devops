# Using a lightweight Python base image
FROM python:3.10-alpine

WORKDIR /app

# Install required utilities
RUN apk add --no-cache apache2-utils dos2unix

# Copy all scripts into the image
COPY . .

# Normalize file formats (just in case) and ensure scripts are executable
RUN dos2unix make-data.py \
    && dos2unix generate-votes.sh \
    && chmod +x generate-votes.sh

# Run the Python script first, then execute the generator script
CMD ["sh", "-c", "python make-data.py && ./generate-votes.sh"]
