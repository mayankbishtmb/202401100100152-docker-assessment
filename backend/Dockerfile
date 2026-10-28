FROM python:3.13-alpine3.22

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY app.py .

USER appuser

EXPOSE 5000

CMD ["python", "app.py"]