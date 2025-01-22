# Proyecto Docker con React, Node.js y Nginx

Este proyecto implementa una arquitectura containerizada usando Docker, que incluye una aplicación frontend en React, una API backend en Node.js y Nginx como proxy inverso. La configuración incluye soporte SSL con certificados autofirmados para comunicación segura.

## Descripción de la Arquitectura

- **Frontend**: Aplicación React servida en el puerto 3000
- **Backend**: API Node.js ejecutándose en el puerto 5000
- **Proxy**: Nginx manejando la terminación SSL y el enrutamiento de peticiones
- **Seguridad**: HTTPS habilitado con certificados autofirmados

## Requisitos Previos

- Servidor Ubuntu (o similar)
- Docker y Docker Compose instalados
- Acceso SSH al servidor

## Estructura del Proyecto

```
.
├── docker-compose.yml
├── next-productivity-API
│   └── Dockerfile
├── next-productivity-app
│   └── react
│       └── next-productivity-app
│           ├── Dockerfile
│           ├── package.json
│           └── src
├── nginx
│   ├── certs
│   │   ├── selfsigned.crt
│   │   └── selfsigned.key
│   └── nginx.conf
```

## Pasos de Instalación

1. **Preparación del Servidor**
   ```bash
   sudo apt update
   sudo apt upgrade
   ```

2. **Instalación de Docker (si no está instalado)**
   ```bash
   sudo apt update
   sudo apt install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   sudo apt install docker-compose
   ```

3. **Configuración del Proyecto**
   ```bash
   # Crear directorios del proyecto
   mkdir -p ~/docker-project/nginx/certs
   mkdir ~/docker-project/next-productivity-app
   mkdir ~/docker-project/next-productivity-API
   ```

4. **Generación del Certificado SSL**
   ```bash
   openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
   -keyout ~/docker-project/nginx/certs/selfsigned.key \
   -out ~/docker-project/nginx/certs/selfsigned.crt \
   -subj "/C=ES/ST=Estado/L=Ciudad/O=Organización/OU=Departamento/CN=localhost"
   ```

## Archivos de Configuración

### Docker Compose
El archivo `docker-compose.yml` define tres servicios:
- Aplicación frontend en React
- API backend en Node.js
- Proxy inverso Nginx

### Configuración de Nginx
La configuración de Nginx incluye:
- Redirección de HTTP a HTTPS
- Configuración de certificados SSL
- Configuración del proxy para las aplicaciones React y Node.js
- Cabeceras CORS para los endpoints de la API

## Construcción y Ejecución

1. **Construir los contenedores:**
   ```bash
   docker-compose build
   ```

2. **Iniciar los servicios:**
   ```bash
   docker-compose up
   ```

3. **Acceder a las aplicaciones:**
   - Frontend: `https://localhost/`
   - API: `https://localhost/tasks/`

## Variables de Entorno

La aplicación React utiliza la siguiente variable de entorno:
- `REACT_APP_BASE_URL`: URL del endpoint de la API (por defecto: https://192.168.1.192/tasks)

## Volúmenes

La API de Node.js utiliza un volumen para persistir datos:
- `tasks.json` se monta en `/data/tasks.json` en el contenedor

## Consideraciones de Seguridad

- El proyecto utiliza certificados SSL autofirmados (no recomendado para producción)
- CORS está configurado para permitir todos los orígenes (modificar según sea necesario)
- Los protocolos SSL están limitados a TLSv1.2 y TLSv1.3

## Solución de Problemas

Si encuentras problemas:

1. **Verificar el estado de Docker:**
   ```bash
   sudo systemctl status docker
   ```

2. **Ver logs de los contenedores:**
   ```bash
   docker-compose logs
   ```

3. **Verificar disponibilidad de puertos:**
   ```bash
   netstat -tuln
   ```

## Cómo Contribuir

1. Haz un fork del repositorio
2. Crea una rama para tu funcionalidad
3. Realiza tus cambios
4. Sube los cambios a la rama
5. Crea un Pull Request


