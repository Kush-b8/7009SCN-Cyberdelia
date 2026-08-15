# Stage 1: Build environment
FROM python:3.9-slim AS builder
WORKDIR /app

# Create a virtual environment and update PATH
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Install dependencies into the venv
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Production runtime (Minimal, Stable, & Secure)
FROM python:3.9-slim
WORKDIR /app

# Update security patches and create a non-root user (SID: 20679)
RUN apt-get update && apt-get upgrade -y && \
    groupadd -g 20679 cybergroup && \
    useradd -u 20679 -g cybergroup -s /usr/sbin/nologin -M cyberuser && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Copy the completely isolated virtual environment from the builder
COPY --from=builder /opt/venv /opt/venv

# Force Python to use the virtual environment
ENV PATH="/opt/venv/bin:$PATH"

# Copy application code
COPY . .

# Strip permissions, making the app directory read/execute only
RUN chown -R cyberuser:cybergroup /app && \
    chmod -R 750 /app

# Drop root privileges globally
USER 20679

# Expose only the internal Flask port
EXPOSE 5000

# Launch the application safely
CMD ["python", "app/app.py"]
