# Etapa 1: Build
FROM eclipse-temurin:17-jdk-alpine AS builder
WORKDIR /app
COPY .mvn/ .mvn/
COPY mvnw pom.xml ./
RUN chmod +x ./mvnw
RUN ./mvnw dependency:go-offline
COPY src ./src
RUN ./mvnw clean package -DskipTests

# Etapa 2: Run
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
RUN addgroup -g 1000 spring && adduser -u 1000 -G spring -s /bin/sh -D spring
USER spring:spring
COPY --from=builder /app/target/*.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
