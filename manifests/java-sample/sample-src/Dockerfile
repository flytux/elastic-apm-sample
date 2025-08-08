# Use an official Maven image as the base image
FROM maven:3.9.9-eclipse-temurin-21-alpine AS build
# Set the working directory in the container
WORKDIR /app
# Copy the pom.xml and the project files to the container
COPY pom.xml .
COPY src ./src
# Build the application using Maven
RUN mvn clean package -DskipTests
# Use an official OpenJDK image as the base image
FROM eclipse-temurin:21-jre-ubi9-minimal
# Set the working directory in the container
WORKDIR /app
# Copy the built JAR file from the previous stage to the container
COPY --from=build /app/target/rolldice.jar .
COPY opentelemetry-javaagent-2.16.0.jar .

ENV OTEL_RESOURCE_ATTRIBUTES="service.name=rolldice,service.instance.id=localhost:8080"
ENV OTEL_EXPORTER_OTLP_ENDPOINT="http://otel-collector:4318"
ENV OTEL_METRIC_EXPORT_INTERVAL=500
ENV OTEL_BSP_SCHEDULE_DELAY=500

# Set the command to run the application
CMD ["java", "-javaagent:opentelemetry-javaagent-2.16.0.jar", "-jar", "rolldice.jar"]
