# ==============================================================================
# IMAGEN BASE: NGINX Alpine
# Usamos Nginx sobre Alpine Linux por ser extremadamente liviano y rápido para servir archivos estáticos
# ==============================================================================
FROM nginx:alpine

# Limpiamos los archivos por defecto que trae la imagen de Nginx
RUN rm -rf /usr/share/nginx/html/*

# Copiamos todos los archivos estáticos del frontend (HTML, CSS, JS) al directorio web de Nginx
COPY . /usr/share/nginx/html

# Indicamos que el servidor Nginx escuchará en el puerto 80 por defecto
EXPOSE 80

# Comando para iniciar Nginx en primer plano (evita que el contenedor se detenga)
CMD ["nginx", "-g", "daemon off;"]
