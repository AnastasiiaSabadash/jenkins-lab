FROM python:3.13-slim

WORKDIR /app

RUN pip install --no-cache-dir Flask

COPY . .

EXPOSE 5000
CMD ["python3", "app.py"]