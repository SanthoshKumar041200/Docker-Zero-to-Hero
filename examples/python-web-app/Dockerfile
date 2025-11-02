# ---------- Stage 1: Build dependencies ----------
FROM python:3.10-slim AS builder

WORKDIR /app

# Install dependencies for building wheels
RUN apt-get update && apt-get install -y --no-install-recommends build-essential

# Copy only dependency file
COPY requirements.txt .

# Create a virtual environment and install dependencies there
RUN python -m venv /venv \
    && /venv/bin/pip install --upgrade pip \
    && /venv/bin/pip install --no-cache-dir -r requirements.txt

# ---------- Stage 2: Run application ----------
FROM python:3.10-slim

WORKDIR /app

# Copy virtual environment from builder
COPY --from=builder /venv /venv

# Copy only your Django project files
COPY devops /app

# Use the venv Python
ENV PATH="/venv/bin:$PATH"

# Expose port
EXPOSE 8000

# Run the Django app
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]




