# syntax=docker/dockerfile:1.4

# Build stage
FROM --platform=$BUILDPLATFORM maven:3.8.5-eclipse-temurin-17 AS builder
WORKDIR /workdir/server

# Copy pom.xml and install dependencies
COPY pom.xml /workdir/server/pom.xml
RUN mvn dependency:go-offline

# Copy source code and build the application
COPY src /workdir/server/src
RUN mvn install

# Production stage: Prepare the application to run
FROM eclipse-temurin:17-jre-focal AS production

# Expose the application port
EXPOSE 8080

# Set working directory for the app
WORKDIR /app

# Copy the application from the build stage
COPY --from=builder /workdir/server/target/*.jar /app/app.jar

# Set the entry point to run the Spring Boot application
ENTRYPOINT ["java", "-jar", "/app/app.jar"]

