# Stage 1: Build the Spring Boot application with Maven and Java 21
FROM maven:3.9.9-eclipse-temurin-21 AS builder

WORKDIR /app

# Copy Maven configuration
COPY pom.xml .

# Copy application source code
COPY src ./src

# Build the application without running tests
RUN mvn clean package -DskipTests

# Stage 2: Run the application with a lightweight Java 21 JRE
FROM eclipse-temurin:21-jre

WORKDIR /app

# Copy the generated JAR from the build stage
COPY --from=builder /app/target/back-end-0.0.1-SNAPSHOT.jar app.jar

# Spring Boot application port
EXPOSE 8080

# Start the application
ENTRYPOINT ["java", "-jar", "app.jar"]
