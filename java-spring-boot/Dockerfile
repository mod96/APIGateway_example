FROM 11-jdk-slim-bullseye AS base

WORKDIR /app

COPY .mvn/ .mvn/
COPY mvnw pom.xml ./
RUN chmod +rwx mvnw && sed -i 's/\r$//' mvnw
RUN ./mvnw dependency:resolve

COPY src ./src

FROM base AS development
CMD ["./mvnw", "spring-boot:run"]

FROM base AS build
RUN ./mvnw package

FROM 11-jre-slim-bullseye as production
EXPOSE 8080
COPY --from=build /app/target/demo-*.jar /production.jar
CMD ["java", "-jar", "/production.jar"]

FROM base as test
CMD ["./mvnw", "test"]
