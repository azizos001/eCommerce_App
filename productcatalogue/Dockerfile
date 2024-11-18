FROM openjdk:8-jre
ADD productcatalogue/target/productcatalogue-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8020
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","app.jar", "server", "app-config.yml"]
