# Étape 1 : builder l'application avec Maven
FROM maven:3.8.6-eclipse-temurin-17 AS build

WORKDIR /app

# Copier les fichiers pom.xml et sources
COPY pom.xml .
COPY src ./src

# Builder le jar (package)
RUN mvn clean package -DskipTests

# Étape 2 : image finale pour exécuter l'application
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

# Copier le jar depuis l'étape de build
COPY --from=build /app/target/*.jar app.jar

# Exposer le port si besoin (exemple : 8080)
EXPOSE 8080

# Commande pour lancer l'application
ENTRYPOINT ["java", "-jar", "app.jar"]
