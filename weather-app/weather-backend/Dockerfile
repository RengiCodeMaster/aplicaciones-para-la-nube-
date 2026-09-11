# ==============================================================================
# ETAPA 1: CONSTRUCCIÓN (Build Stage)
# Usamos una imagen de Maven con Java 17 para compilar el proyecto Spring Boot
# ==============================================================================
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Establecemos el directorio de trabajo dentro del contenedor para la compilación
WORKDIR /app

# Copiamos primero el archivo de dependencias (pom.xml) para aprovechar el caché de Docker
COPY pom.xml .

# Descargamos las dependencias sin compilar aún el código fuente (optimiza tiempos de build)
RUN mvn dependency:go-offline -B

# Copiamos todo el código fuente del backend al contenedor
COPY src ./src

# Compilamos y empaquetamos la aplicación generando el archivo .jar (omitiendo tests para mayor rapidez)
RUN mvn clean package -DskipTests

# ==============================================================================
# ETAPA 2: EJECUCIÓN (Runtime Stage)
# Usamos una imagen JRE ligera (Alpine) para mantener la imagen final pequeña y segura
# ==============================================================================
FROM eclipse-temurin:17-jre-alpine

# Definimos el directorio de trabajo en la imagen final
WORKDIR /app

# Copiamos únicamente el archivo JAR generado en la etapa anterior (builder)
COPY --from=builder /app/target/weather-api-0.0.1-SNAPSHOT.jar app.jar

# Informamos a Docker que el contenedor escuchará en el puerto 8080
EXPOSE 8080

# Definimos las opciones de la Máquina Virtual de Java (JVM) para optimizar memoria
ENV JAVA_OPTS="-Xms128m -Xmx512m"

# Comando final que se ejecuta al arrancar el contenedor
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
