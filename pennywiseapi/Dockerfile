# ==========================================
# STAGE 1: Build the application
# ==========================================
FROM maven:3.9.6-eclipse-temurin-21-alpine AS builder
WORKDIR /app

# Step 1: Copy ONLY the pom.xml first
COPY pom.xml .

# Step 2: Download all dependencies.
# Because the pom.xml rarely changes, Docker will CACHE this layer!
RUN mvn dependency:go-offline

# Step 3: Now copy the actual source code
COPY src ./src

# Step 4: Build the application (runs fast because dependencies are already cached)
RUN mvn clean package -DskipTests

# ==========================================
# STAGE 2: Run the application
# ==========================================
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# Copy only the built .jar file from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose port and run
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]