🧱 Use Case: Python Web App (e.g., FastAPI/Flask) with native dependencies
Let’s say your app uses dependencies like uvicorn, pandas, or psycopg2 that require compilation.

✅ Multi-Stage Python Dockerfile
dockerfile
Copy
Edit
# -------- Stage 1: Build Stage --------
FROM python:3.11-slim AS builder

# Install build dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    libpq-dev \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Install Python packages in a virtualenv
COPY requirements.txt .
RUN python -m venv /opt/venv && \
    /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install -r requirements.txt

# -------- Stage 2: Final Stage --------
FROM python:3.11-slim

# Copy only the virtual environment from builder
COPY --from=builder /opt/venv /opt/venv

ENV PATH="/opt/venv/bin:$PATH"

WORKDIR /app
COPY . .

# Run your app (adjust to your app entry point)
CMD ["python", "main.py"]
🧠 What this does:
Stage	What happens
Stage 1 (builder)	Installs build tools & Python packages in a virtual environment
Stage 2 (final)	Copies just the pre-built environment & source code — no compilers or dev tools

📦 Result:
Final image is clean and lightweight

Build dependencies (like gcc, build-essential, etc.) don’t end up in your final container

Safer, faster to pull, and easier to maintain

🛠 Tips:
If you don’t use a virtual environment, you can also pip install --target and copy that instead.

Works great for ML/AI models too — let me know if you want a PyTorch or TensorFlow example!

Would you like this adapted for a FastAPI app with uvicorn or for a machine learning project with large model files?
