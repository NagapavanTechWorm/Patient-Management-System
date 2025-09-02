# ====== Build Stage ======
FROM maven:3.9.9-eclipse-temurin-21 AS builder
WORKDIR /app

# Copy pom.xml first and download dependencies (better caching)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the project (jar or war)
RUN mvn clean package -DskipTests

# ====== Runtime Stage ======
FROM openjdk:21-jdk AS runner
WORKDIR /app

# Copy the built artifact (jar/war) without hardcoding the name
COPY --from=builder /app/target/*.jar app.jar

# Expose port
EXPOSE 4000

# Run jar
ENTRYPOINT ["java", "-jar", "app.jar"]
