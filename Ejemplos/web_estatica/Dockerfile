# Usa la imagen base de nginx
FROM nginx:latest

# Copia los archivos de tu página al directorio donde nginx los sirve
COPY . /usr/share/nginx/html

# Exponer el puerto 80
EXPOSE 80

# Mantén nginx corriendo en primer plano
CMD ["nginx", "-g", "daemon off;"]
