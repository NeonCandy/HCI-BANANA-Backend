FROM python:3.10-slim

# Use root user explicitly (default in most images)
USER root

# Set environment variables for database configuration
ARG DB_NAME
ARG DB_USER
ARG DB_PASS
ARG DB_HOST
ARG DB_PORT

ENV DB_NAME=$DB_NAME
ENV DB_USER=$DB_USER
ENV DB_PASS=$DB_PASS
ENV DB_HOST=$DB_HOST
ENV DB_PORT=$DB_PORT

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    python3-dev \
    default-libmysqlclient-dev \
    libpq-dev \
    build-essential \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Set work directory
WORKDIR /app

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Ensure the Python executable has the correct permissions
RUN chmod +x /usr/local/bin/python

# Ensure that the app directory and its contents are readable and executable
RUN chmod -R 755 /app

# Run Django migrations
RUN python manage.py migrate

# Expose the port Django runs on
EXPOSE 8000

# Start Django server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
