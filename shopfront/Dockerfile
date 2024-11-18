# Step 1: Use a Maven image to build the Java application
FROM maven:3.8.6-openjdk-11 AS build

# Set the working directory inside the container
WORKDIR /app

# Copy the pom.xml and the source code (Java files) into the container
COPY pom.xml .
COPY src ./src

# Build the JAR file inside the container using Maven
RUN mvn clean install

# Step 2: Use a lighter JRE image to run the application
FROM openjdk:11-jre-slim

# Set the working directory inside the container
WORKDIR /app

# Copy the built JAR file from the 'build' stage to the final image
COPY --from=build /app/target/shopfront-0.0.1-SNAPSHOT.jar app.jar

# Expose the port the app runs on
EXPOSE 8080

# Command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
