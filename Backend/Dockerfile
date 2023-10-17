# FROM eclipse-temurin:17-jdk-jammy
#
# WORKDIR /app
#
# COPY .mvn/ .mvn
# COPY mvnw pom.xml ./
# RUN ./mvnw dependency:resolve
#
# COPY src ./src
#
# CMD ["./mvnw", "spring-boot:run"]

# FROM eclipse-temurin:17-jdk-alpine
# WORKDIR /app
# EXPOSE 8080
# COPY Tipout-0.0.1-SNAPSHOT.jar Tipout-0.0.1-SNAPSHOT.jar
# ENTRYPOINT ["java","-jar","Tipout-0.0.1-SNAPSHOT.jar"]

# FROM --platform=$BUILDPLATFORM maven:3.8.5-eclipse-temurin-17 AS builder
# WORKDIR /workdir/server
# COPY pom.xml /workdir/server/pom.xml
# RUN mvn dependency:go-offline
#
# COPY src /workdir/server/src
# RUN mvn install
#
# FROM builder as dev-envs
#
# RUN <<EOF
# apt-get update
# apt-get install -y git
# EOF
#
# RUN <<EOF
# useradd -s /bin/bash -m vscode
# groupadd docker
# usermod -aG docker vscode
# EOF
# # install Docker tools (cli, buildx, compose)
# COPY --from=gloursdocker/docker / /
# CMD ["mvn", "spring-boot:run"]
#
# FROM builder as prepare-production
# RUN mkdir -p target/dependency
# WORKDIR /workdir/server/target/dependency
# RUN jar -xf ../*.jar
#
# FROM eclipse-temurin:17-jre-focal
#
# EXPOSE 8080
# VOLUME /tmp
# ARG DEPENDENCY=/workdir/server/target/dependency
# COPY --from=prepare-production ${DEPENDENCY}/BOOT-INF/lib /app/lib
# COPY --from=prepare-production ${DEPENDENCY}/META-INF /app/META-INF
# COPY --from=prepare-production ${DEPENDENCY}/BOOT-INF/classes /app
# ENTRYPOINT ["java","-cp","app:app/lib/*","com.company.project.Application"]

#Build stage

FROM gradle:7.6.1 AS BUILD
WORKDIR /usr/app/
COPY . .
RUN gradle build

# Package stage

FROM openjdk:latest
ENV JAR_NAME=Tipout-0.0.1-SNAPSHOT.jar
ENV APP_HOME=/usr/app
WORKDIR $APP_HOME
COPY --from=BUILD $APP_HOME .
EXPOSE 8080
ENTRYPOINT exec java -jar $APP_HOME/build/libs/$JAR_NAME