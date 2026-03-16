# ---------- Builder Stage ----------
FROM python:3.11-slim AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --user -r requirements.txt

# ---------- Runtime Stage ----------
FROM python:3.11-alpine

WORKDIR /app

COPY --from=builder /root/.local /root/.local

COPY app.py .

ENV PATH=/root/.local/bin:$PATH

RUN adduser -D appuser
USER appuser

CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]