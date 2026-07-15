# 🎓 moodle-docker: Plataforma de educación virtual Moodle con Docker

[![GitHub](https://img.shields.io/badge/GitHub-Repositorio-blue)](https://github.com/JLalib/moodle-docker)
[![Docker](https://img.shields.io/badge/Docker-Moodle-blue)](https://hub.docker.com/r/bitnami/moodle)
[![MariaDB](https://img.shields.io/badge/Base%20Datos-MariaDB-blue)](https://hub.docker.com/r/bitnami/mariadb)
[![License](https://img.shields.io/badge/Licencia-MIT-green)](https://github.com/JLalib/moodle-docker/blob/main/LICENSE)

## 📋 Descripción general

Moodle es un sistema de gestión de aprendizaje (LMS) de código abierto ampliamente utilizado para crear sitios web con cursos en línea. Este repositorio proporciona una configuración Docker Compose para desplegar Moodle con MariaDB usando las imágenes oficiales de Bitnami, siguiendo el tutorial de Genbyte.

Con esta configuración, tendrás tu plataforma de educación virtual lista para usar en minutos, completa con base de datos y persistencia de datos.

## ✨ Características principales

- **Gestión de cursos completa**: Crear, organizar y entregar cursos en línea con recursos, actividades y evaluaciones
- **Aulas virtuales**: Foros, chats, wikis, talleres y actividades colaborativas
- **Evaluación y calificaciones**: Quizzes, tareas, talleres y libros de calificaciones avanzados
- **Multilingüe**: Disponible en más de 100 idiomas
- **Gestión de usuarios y roles**: Sistemas de permisos flexibles para administradores, profesores y estudiantes
- **Integración de plugins**: Amplía funcionalidades con el directorio de plugins de Moodle
- **Responsive design**: Accesible desde cualquier dispositivo (desktop, tablet, móvil)
- **Seguridad**: Actualizaciones regulares y mejores prácticas de seguridad
- **Escalabilidad**: Fácil de escalar con balanceo de carga y clustering (configuración avanzada)

## 📋 Requisitos del sistema

- Docker y Docker Compose instalados
- Al menos 2 GB de RAM disponibles (4 GB recomendado para uso concurrente)
- Espacio en disco suficiente para los datos de Moodle y archivos subidos por usuarios
- Puerto 8080 disponible para acceso HTTP
- Puerto 8443 disponible para acceso HTTPS (opcional, si se configura SSL)

## 🐳 Instalación

### Docker Compose (Método recomendado)

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
version: '3.3'

services:

  mariadb:
    image: bitnami/mariadb:latest
    container_name: moodle-db
    environment:
      - ALLOW_EMPTY_PASSWORD=no
      - MARIADB_USER=bmoodle_user
      - MARIADB_PASSWORD=your_m…here
      - MARIADB_DATABASE=bitnami_moodle
    volumes:
      - ./moodle_db:/var/lib/mysql

  moodle:
    image: bitnami/moodle:latest
    container_name: moodle
    ports:
      - "8080:8080"
      - "8443:8443"
    environment:
      - ALLOW_EMPTY_PASSWORD=no
      - MOODLE_DATABASE_USER=bmoodle_user
      - MOODLE_DATABASE_PASSWORD=your_m…here
      - MOODLE_DATABASE_NAME=bitnami_moodle
      - MOODLE_USERNAME=admin
      - MOODLE_PASSWORD=your_admin_password_here
    volumes:
      - ./moodle_data:/bitnami/moodle
      - ./moodledata_data:/bitnami/moodledata
    depends_on:
      - mariadb
```

Luego, inicia el servicio:

```bash
docker compose up -d
```

## ⚙️ Configuración

Antes de iniciar los contenedores, debes editar el archivo `docker-compose.yml` para establecer tus contraseñas:

1. **MARIADB_PASSWORD**: Contraseña para el usuario de MariaDB
2. **MOODLE_DATABASE_PASSWORD**: Contraseña para la conexión de Moodle a la base de datos
3. **MOODLE_PASSWORD**: Contraseña para el usuario administrador de Moodle

💡 Consejo: usa contraseñas seguras y diferentes para cada credencial.

## 🚀 Primeros pasos

1. Asegúrate de tener Docker y Docker Compose instalados en tu sistema
2. Clona este repositorio o copia el archivo `docker-compose.yml` a tu servidor
3. Edita el archivo `docker-compose.yml` y reemplaza:
   - `your_m…here` por una contraseña segura para el usuario de MariaDB
   - `your_admin_password_here` por una contraseña segura para el administrador de Moodle
4. Ejecuta `docker compose up -d` para iniciar los contenedores
5. Abre tu navegador en `http://localhost:8080` (o tu dominio si lo has configurado)
6. Completa el proceso de instalación inicial de Moodle:
   - Selecciona el idioma
   - Confirma las rutas de los directorios
   - La configuración de la base de datos debería estar prellenada si usaste los mismos valores
   - Crea tu cuenta de administrador (si no la especificaste en las variables de entorno)
   - Configura el nombre de tu sitio y otras opciones básicas
   - Finaliza la instalación

## 💡 Casos de uso

- **Instituciones educativas**: Escuelas, colegios y universidades para impartir cursos en línea
- **Capacitación corporativa**: Formación y desarrollo de empleados en empresas
- **Cursos abiertos (MOOCs)**: Ofrecer cursos gratuitos o de pago a una audiencia global
- **Organizaciones sin fines de lucro**: Capacitación de voluntarios y miembros
- **Autoaprendizaje**: Crear tu propio entorno de aprendizaje personal
- **Formación técnica**: Cursos de programación, diseño, marketing y habilidades profesionales

## 🔒 Acceso remoto seguro (opcional)

Si deseas acceder a Moodle desde fuera de tu red local de forma segura, puedes usar un proxy inverso como Nginx Proxy Manager o Traefik para obtener un certificado gratuito de Let's Encrypt.

### Configuración Caddyfile (ejemplo)
```caddy
moodle.tudominio.com {
    reverse_proxy localhost:8080
    encode gzip
}
```

### Pasos para acceso seguro
1. Instala y configura Caddy, Nginx Proxy Manager o Traefik en tu servidor
2. Obtén un certificado SSL gratuito de Let's Encrypt (Caddy lo hace automáticamente)
3. Configura el proxy inverso para apuntar a `localhost:8080`
4. Accede a Moodle a través de tu dominio seguro (https://moodle.tudominio.com)

## 🛠️ Gestión y mantenimiento

### Ver registros
```bash
docker compose logs -f
```

### Actualizar a la última versión
```bash
docker compose pull
docker compose up -d
```

### Reiniciar servicios
```bash
docker compose restart
```

### Ver estado de salud
```bash
docker compose ps
```

### Limpiar y comenzar desde cero
```bash
docker compose down -v  # Elimina contenedores, redes y volúmenes
# Luego vuelve a levantar con: docker compose up -d
```

## 📝 Licencia

Este proyecto se basa en [Bitnami Moodle](https://github.com/bitnami/containers/tree/main/bitnami/moodle) y [Bitnami MariaDB](https://github.com/bitnami/containers/tree/main/bitnami/mariadb) que están licenciados bajo la [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). La configuración y documentación proporcionada aquí está bajo la [Licencia MIT](https://github.com/JLalib/moodle-docker/blob/main/LICENSE).

---

> ✨ **Nota**: Este repositorio contiene la configuración Docker y documentación extraída del tutorial de Genbyte: [MOODLE en Docker. Tu plataforma cursos online Moodle en DOCKER 🐳](https://genbyte.blogspot.com/2024/04/moodle-en-docker-tu-plataforma-cursos_2.html) y utiliza la composición proporcionada en [https://github.com/JLalib/docker-moodle/blob/main/docker-compose.yml](https://github.com/JLalib/docker-moodle/blob/main/docker-compose.yml).