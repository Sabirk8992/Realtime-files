write a complex dockerfile for realtime use with reference to above

Sure, here's an example of a more complex Dockerfile for a real-time production use case:

```
# Use an official Python runtime as the base image
FROM python:3.12-alpine

# Set the working directory
WORKDIR /usr/src/app

# Copy the requirements file and install the dependencies
COPY requirements.txt .
RUN apk add --no-cache --virtual .build-deps gcc musl-dev \
    && pip install --no-cache-dir -r requirements.txt \
    && apk del .build-deps

# Copy the application code
COPY app.py .
COPY templates/ templates/
COPY static/ static/

# Set environment variables
ENV FLASK_APP=app.py
ENV FLASK_ENV=production

# Expose the port the application will run on
EXPOSE 5000

# Create a non-root user
RUN adduser -D myuser
USER myuser

# Run the command to start the application
CMD ["flask", "run", "--host=0.0.0.0"]

```
This Dockerfile uses an official Python runtime as the base image, copies the application code and dependencies, sets environment variables, exposes a port, and creates a non-root user before running the command to start the application.

It will create a working directory '/usr/src/app' and copies the application files and requirements file into it. And will install all the dependencies mentioned in requirements.txt, to install production dependencies. It will expose port 5000 and uses 'flask run --host=0.0.0.0' command to run the flask application with the host as 0.0.0.0 for external access.

It is important to mention that this is just an example and the actual requirements may vary depending on the specific use case and application. It's always a good practice to test the container images in a testing environment before deploying them in production.
