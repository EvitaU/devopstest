# Stage 1: Build stage
FROM python:3.9-slim-buster AS builder

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on


WORKDIR /install

COPY requirements.txt .

RUN pip install --prefix=/install --no-cache-dir -r requirements.txt

# Stage 2: Runtime stage
FROM python:3.9-slim-buster

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

RUN useradd -ms /bin/bash appuser

WORKDIR /app

COPY --from=builder /install /usr/local

COPY . .

RUN chown -R appuser:appuser /app

USER appuser

EXPOSE 5000

# Run the application
CMD ["flask", "run", "--host=0.0.0.0", "--port=5000"]