# Stage 1: Build
FROM python:3.11-slim AS builder

WORKDIR /app

COPY pyproject.toml ./
RUN pip install --no-cache-dir .[test] uvicorn
COPY . .

# Устанавливаем пакет в виртуальном окружении или локально (в /app)

# Stage 2: Production image (только копируем установленное)
FROM python:3.11-slim

RUN useradd -m appuser
WORKDIR /app

# Копируем установленные зависимости и код из builder-стадии
COPY --from=builder /usr/local/lib/python3.11/site-packages /usr/local/lib/python3.11/site-packages
COPY --from=builder /usr/local/bin/uvicorn /usr/local/bin/uvicorn
COPY --from=builder /app /app

USER appuser

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8040"]
