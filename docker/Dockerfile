# Dockerfile
FROM amazoncorretto:17-alpine
RUN apk add --no-cache tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
VOLUME /tmp
COPY GJ-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-XX:MaxRAMPercentage=75.0","-Djasypt.encryptor.password=${JASYPT_PASSWORD}","-jar","/app.jar"]
