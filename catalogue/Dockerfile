# Use a smaller base image
FROM python:3.9-alpine

WORKDIR /app
COPY . /app

# Install Gunicorn and other dependencies
RUN pip install --no-cache-dir -r requirements.txt gunicorn

# Create a non-root user and switch to that user
RUN adduser -D nonrootuser && chown -R nonrootuser /app
USER nonrootuser

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
