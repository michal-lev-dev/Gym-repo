# Stage 1: Build the application
FROM maven:3.9-amazoncorretto-17 AS builder

# Yazar bilgisi
LABEL authors="Mehmet Furkan Kaya <furkan.36kaya@gmail.com>"

WORKDIR /app

# Copy the project files to the container
COPY ./pom.xml ./pom.xml

# Download all required dependencies into one layer
RUN mvn dependency:go-offline -B

# Copy your other files
COPY ./src ./src

# Build the application
RUN mvn package -DskipTests

# Stage 2: Create the runtime image
FROM amazoncorretto:17-alpine

# Set the working directory
WORKDIR /app

# Create the file storage directory for the non-root runtime user
RUN mkdir -p /app/videos && chown -R 10001:10001 /app

# Copy the built artifact from the builder stage
COPY --from=builder --chown=10001:10001 /app/target/*.jar /app/app.jar

# Run as a non-root user
USER 10001:10001

# Run the application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
