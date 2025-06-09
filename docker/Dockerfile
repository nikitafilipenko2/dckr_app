# Stage 1
FROM python:3.11-slim as builder

WORKDIR /app

RUN pip install --no-cache-dir .

COPY pyproject.toml ./

RUN pip install .[test]

COPY . .

# Stage 2
FROM python:3.11-slim

RUN useradd -m appuser

WORKDIR /app
COPY --from=builder /app /app

USER appuser

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8040"]
