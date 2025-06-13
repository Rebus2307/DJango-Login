# Sistema de Login con Django

Este proyecto implementa un sistema básico de autenticación de usuarios utilizando Django y PostgreSQL, todo containerizado con Docker.

## 🚀 Características

- **Registro de usuarios** con validación de formularios
- **Inicio y cierre de sesión** 
- **Listado de usuarios registrados** (solo para usuarios autenticados)
- **Interfaz responsive** con Bootstrap 5
- **Base de datos PostgreSQL** 
- **Containerización completa** con Docker

## 📋 Prerrequisitos

- Docker
- Docker Compose

## 🛠️ Instalación y Configuración

### 1. Clonar el repositorio
```bash
git clone <url-del-repositorio>
cd django-login-system
```

### 2. Ejecutar el proyecto
```bash
docker-compose up --build
```

El comando anterior:
- Construye las imágenes de Docker
- Inicia los contenedores (Django + PostgreSQL)
- Ejecuta las migraciones automáticamente
- Inicia el servidor de desarrollo

### 3. Acceder a la aplicación
Abre tu navegador y ve a: `http://localhost:8000`

## 🗂️ Estructura del Proyecto

```
django-login-system/
├── app/
│   ├── templates/
│   │   ├── registration/
│   │   │   ├── login.html
│   │   │   └── registro.html
│   │   └── usuarios/
│   │       └── lista.html
│   ├── static/          # Archivos CSS, JS, imágenes
│   ├── forms.py         # Formularios personalizados
│   ├── views.py         # Lógica de las vistas
│   ├── urls.py          # Configuración de rutas
│   ├── settings.py      # Configuración de Django
│   └── models.py        # Modelos de datos (expandible)
├── docker-compose.yml
├── Dockerfile
└── requirements.txt
```

## 🔧 Cómo crear este proyecto desde cero

### Paso 1: Crear el proyecto Django
```bash
# Crear directorio del proyecto
mkdir django-login-system
cd django-login-system

# Crear entorno virtual (opcional para desarrollo local)
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate

# Instalar Django
pip install django psycopg2-binary

# Crear proyecto Django
django-admin startproject app .
```

### Paso 2: Configurar Docker
Crear `Dockerfile`:
```dockerfile
FROM python:3.9

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD python manage.py migrate && python manage.py runserver 0.0.0.0:8000
```

Crear `docker-compose.yml`:
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - db
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

### Paso 3: Configurar settings.py
Modificar la configuración de base de datos:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ.get('POSTGRES_DB', 'postgres'),
        'USER': os.environ.get('POSTGRES_USER', 'postgres'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD', 'password'),
        'HOST': 'db',
        'PORT': '5432',
    }
}
```

### Paso 4: Crear formularios personalizados
En `forms.py`, crear formularios para registro y login con validación y estilo Bootstrap.

### Paso 5: Crear vistas
En `views.py`, implementar las vistas para:
- Página de inicio
- Registro de usuarios
- Login de usuarios
- Listado de usuarios

### Paso 6: Configurar URLs
En `urls.py`, definir las rutas para todas las vistas.

### Paso 7: Crear templates
Crear los archivos HTML con Bootstrap para:
- Login (`templates/registration/login.html`)
- Registro (`templates/registration/registro.html`)
- Lista de usuarios (`templates/usuarios/lista.html`)

### Paso 8: Ejecutar el proyecto
```bash
docker-compose up --build
```

## 📱 Uso de la Aplicación

### Páginas disponibles:
- `/` - Página de inicio
- `/registro/` - Registro de nuevos usuarios
- `/login/` - Inicio de sesión
- `/logout/` - Cierre de sesión
- `/usuarios/` - Lista de usuarios (requiere autenticación)
- `/admin/` - Panel de administración de Django

### Flujo de usuario:
1. **Registro**: Los nuevos usuarios pueden crear una cuenta
2. **Login**: Usuarios registrados pueden iniciar sesión
3. **Dashboard**: Usuarios autenticados ven información personalizada
4. **Lista de usuarios**: Solo usuarios autenticados pueden ver todos los usuarios registrados

## 🔐 Seguridad

- Validación de formularios del lado servidor
- Protección CSRF en todos los formularios
- Autenticación requerida para páginas sensibles
- Validación de contraseñas con los validadores de Django

## 🚀 Expansiones Futuras

Este proyecto está preparado para agregar:
- Modelos personalizados en `models.py`
- Panel de administración en `admin.py`
- APIs REST
- Perfiles de usuario
- Roles y permisos
- Recuperación de contraseñas

## 🛠️ Comandos Útiles

```bash
# Ejecutar el proyecto
docker-compose up --build

# Detener el proyecto
docker-compose down

# Ver logs
docker-compose logs

# Ejecutar comandos de Django dentro del contenedor
docker-compose exec web python manage.py createsuperuser
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py collectstatic
```

## 📝 Notas

- El proyecto está configurado para desarrollo (DEBUG=True)
- Para producción, cambiar las configuraciones de seguridad
- La base de datos se reinicia cada vez que se elimina el contenedor
- Los archivos estáticos se sirven directamente por Django (no recomendado para producción)

## 🤝 Contribuir

1. Fork el proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request