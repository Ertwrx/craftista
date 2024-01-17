# Stage 1: Build the application
FROM schoolofdevops/maven:spring AS builder

WORKDIR /app
COPY . /app

# Running Maven build as non-root user
RUN mvn clean package -Dmaven.compiler.target=1.8 -Dmaven.compiler.source=1.8

# Stage 2: Use Distroless Java 17 non-root image for minimal runtime
FROM gcr.io/distroless/java17-debian12:nonroot

WORKDIR /app

# Copying the JAR file from the builder stage
COPY --from=builder /app/target/voting-0.0.1-SNAPSHOT.jar /app/app.jar

EXPOSE 8080
CMD ["app.jar"]
