
## 1. Bases de Datos en Django

### ¿Qué función cumple una base de datos dentro de una aplicación Django?
La base de datos actúa como la **capa de persistencia** del sistema. Su función principal es almacenar, organizar y gestionar de manera estructurada toda la información que la aplicación necesita retener a largo plazo. 

En un entorno web, esto incluye:
* Registros y credenciales de usuarios.
* Contenido dinámico (artículos, productos, comentarios, etc.).
* Configuraciones globales del sitio y logs de actividad.

*Sin una base de datos, cualquier información introducida o modificada por los usuarios se perdería inmediatamente al reiniciar o detener el servidor web.*

### ¿Qué sistemas de bases de datos relacionales soporta Django por defecto?
Django incluye soporte oficial y nativo a través de sus propios *backends* integrados para los siguientes sistemas de gestión de bases de datos relacionales (RDBMS):
1. **PostgreSQL**
2. **MySQL**
3. **SQLite**
4. **Oracle**
5. **MariaDB** (soportado nativamente a través del backend de MySQL o de forma específica según la versión)

### ¿Cuál es el motor de base de datos que se utiliza por defecto al crear un nuevo proyecto? ¿Por qué crees que es ese?
El motor configurado por defecto es **SQLite** (creando automáticamente un archivo llamado `db.sqlite3` en la raíz del proyecto).

**Razones de esta elección:**
* **Cero Configuración:** No requiere instalar un servidor de bases de datos independiente, gestionar puertos, ni configurar usuarios o contraseñas. Funciona de inmediato.
* **Portabilidad:** Al ser un único archivo local, permite mover, clonar o compartir el proyecto completo de manera muy sencilla entre diferentes desarrolladores.
* **Ligereza en Desarrollo:** Es la opción ideal para las etapas iniciales de diseño, maquetación del proyecto y ejecución de pruebas unitarias sin sobrecargar el sistema de trabajo.

---

## 2. ORM en Django

### ¿Qué es un ORM y cómo se diferencia de escribir sentencias SQL manualmente?
**ORM** significa *Object-Relational Mapping* (Mapeo Objeto-Relacional). Es una herramienta tecnológica que permite interactuar con bases de datos relacionales utilizando el paradigma de **Programación Orientada a Objetos (POO)** en lugar de escribir código de bases de datos directamente.

| Criterio | Escribir Sentencias SQL Manualmente | Usar el ORM de Django |
| :--- | :--- | :--- |
| **Sintaxis** | Se redactan cadenas de texto con instrucciones estrictas (`SELECT * FROM tabla;`). | Se utilizan métodos nativos de Python (`Modelo.objects.all()`). |
| **Dependencia** | Está fuertemente ligado al dialecto específico del motor (Syntax SQL de MySQL vs PostgreSQL). | Es agnóstico al motor; el ORM traduce el código Python al SQL correcto según la configuración. |
| **Seguridad** | Requiere sanitizar manualmente cada entrada para evitar vulnerabilidades. | Protege y parametriza las consultas de forma automática. |

### Menciona al menos dos ventajas de usar el ORM de Django
1. **Independencia del Motor (Portabilidad):** Permite cambiar el motor de base de datos (por ejemplo, de SQLite en desarrollo a PostgreSQL en producción) modificando únicamente unas líneas en el archivo `settings.py`, sin necesidad de alterar una sola línea de código de las consultas.
2. **Seguridad y Mitigación de Inyecciones SQL:** El ORM filtra y parametriza las consultas por defecto de forma interna. Al abstraer la inserción de variables en cadenas SQL, bloquea los vectores de ataque más comunes de inyección de código malicioso.

### Explica qué significa que una clase modelo en Python represente una tabla en la base de datos
Significa que el ORM establece una equivalencia directa y exacta entre las estructuras del código estructurado en Python y el esquema físico de almacenamiento de la base de datos. 

Esta equivalencia se divide de la siguiente manera:
* **La Clase:** Define la estructura de la entidad (ej. `class Libro`) y da origen a una **Tabla** física (ej. `app_libro`).
* **Los Atributos:** Cada variable de la clase (ej. `titulo = models.CharField()`) define una **Columna** con sus respectivos tipos de datos y restricciones.
* **Las Instancias:** Cada objeto individual creado a partir de la clase representa una **Fila (o registro)** guardada dentro de dicha tabla.

---

## 3. Migraciones

### ¿Qué son las migraciones en Django y por qué son importantes?
Las migraciones son archivos de Python autogenerados que actúan como un **sistema de control de versiones para el esquema de la base de datos**. Cada vez que se modifica un modelo, las migraciones registran cómo debe evolucionar la base de datos para adaptarse a esos cambios.

**Por qué son importantes:**
* Permiten modificar la estructura de las tablas (añadir columnas, borrar tablas) de forma incremental sin poner en riesgo los datos existentes.
* Automatizan la creación de las sentencias de actualización de base de datos (`ALTER TABLE`), asegurando que todos los entornos (desarrollo, pruebas y producción) tengan exactamente la misma estructura.

### Qué comandos se utilizan para:
* **Crear una nueva migración a partir de cambios en los modelos:**
  ```bash
  python manage.py makemigrations

* **Aplicar las migraciones a la Base de Datos:**
    ```bash
  python manage.py migrate

### Consultas básicas en el ORM
```bash
from django.db import models

class Libro(models.Model):
    titulo = models.CharField(max_length=100)
    autor = models.CharField(max_length=50)
    publicado = models.BooleanField(default=True)



todos_los_libros = Libro.objects.all() #Obtener todos los libros.
libros_cervantes = Libro.objects.filter(autor="Cervantes") #Filtrar los libros por autor igual a "Cervantes".
libro_especifico = Libro.objects.get(id=1) #Obtener un libro específico por su id.



