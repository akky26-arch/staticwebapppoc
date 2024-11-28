FROM maven as build
WORKDIR /app
COPY . .
RUN mvn clean install

FROM eclipse-temurin:21
WORKDIR /app
COPY --from=build /app/target/auth-api-0.0.1-SNAPSHOT.jar .
EXPOSE 8080
CMD ["java", "-jar", "auth-api-0.0.1-SNAPSHOT.jar"]