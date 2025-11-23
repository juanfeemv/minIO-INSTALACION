````markdown
# 🚀 MinIO Python Client en Raspberry Pi

Este proyecto implementa un cliente de Python para interactuar con un servidor **MinIO** (S3 Compatible) alojado en Docker sobre una Raspberry Pi. Permite generar datos con Pandas, guardar archivos CSV y subirlos automáticamente a un bucket.

---

## 📋 Requisitos Previos

- **Docker** instalado en tu Raspberry Pi o servidor.
- **Python 3.x** instalado.

---

## 🛠️ 1. Configuración de MinIO (Docker)

Para iniciar el servidor MinIO con persistencia de datos y credenciales personalizadas, sigue estos pasos en tu terminal:

### 1.1 Crear directorio para persistencia de datos
```bash
mkdir -p ~/minIO/data
````

### 1.2 Ejecutar contenedor

Ejecuta el siguiente comando para levantar MinIO (reemplaza usuario/contraseña si es necesario):

```bash
sudo docker run -d \
  -p 9000:9000 -p 9001:9001 \
  -e "MINIO_ROOT_USER=menav" \
  -e "MINIO_ROOT_PASSWORD=prueba" \
  -v ~/minIO/data:/data \
  --name minio_local \
  minio/minio server /data --console-address ":9001"
```

  * **Consola Web:** [http://localhost:9001](https://www.google.com/search?q=http://localhost:9001) (o la IP de tu Raspberry).
  * **API Endpoint:** [http://localhost:9000](https://www.google.com/search?q=http://localhost:9000)

-----

## 🐍 2. Configuración del Entorno Python

En sistemas basados en Debian (como Raspberry Pi OS), se recomienda usar un entorno virtual (`venv`) para evitar conflictos con el sistema.

### 2.1 Crear el entorno virtual

```bash
python3 -m venv venv
```

### 2.2 Activar el entorno virtual

```bash
source venv/bin/activate
```

> *Verás `(venv)` al inicio de tu terminal indicando que el entorno está activo.*

### 2.3 Instalar dependencias

```bash
pip install boto3 pandas python-dotenv
```

-----

## ⚙️ 3. Configuración del Proyecto

Crea un archivo llamado `.env` en la raíz del proyecto para guardar tus credenciales de forma segura.

**Archivo `.env`:**

```ini
MINIO_ENDPOINT=http://localhost:9000
MINIO_ROOT_USER=menav
MINIO_ROOT_PASSWORD=prueba
BUCKET_NAME=mi-bucket
```

-----

## 📂 4. Estructura del Código

El proyecto consta de dos scripts principales. Asegúrate de tenerlos en la carpeta del proyecto:

  * **`minio_client.py`**: Maneja la conexión y autenticación con MinIO usando la librería `boto3`.
  * **`main.py`**: Script principal que genera un DataFrame de prueba con Pandas, crea un archivo CSV local y lo sube al bucket configurado.

-----

## ▶️ 5. Ejecución

### 5.1 Activar el entorno virtual (si no está activo)

```bash
source venv/bin/activate
```

### 5.2 Ejecutar el script principal

```bash
python main.py
```

### Resultado esperado

Si todo está configurado correctamente, verás una salida similar a esta:

```text
Archivo local creado: data.csv
Archivo subido a MinIO en el bucket: mi-bucket
```
