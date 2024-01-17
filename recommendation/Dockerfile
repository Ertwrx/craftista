# Stage 1: Build Stage
FROM golang:latest AS builder

# Set a build argument for the application name (can be overridden during build)
ARG APP_NAME=./app

# Set the working directory inside the container
WORKDIR /app

# Copy go.mod and go.sum files to the working directory
COPY go.mod go.sum ./

# Download and install dependencies
RUN go mod download

# Copy the source code to the working directory
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o ${APP_NAME} .

# Stage 2: Final Stage
FROM alpine:latest

# Create and set the working directory inside the container
WORKDIR /app

# Copy the binary from the builder stage
COPY --from=builder /app/${APP_NAME} .

# Create a non-root user
RUN adduser -D -u 1001 myuser

# Set the user as the default user
USER myuser

# Expose the application port
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost:8080/health || exit 1

# Default environment variables
ENV APP_PORT=8080

# Default command to run the application
CMD ["./app"]
