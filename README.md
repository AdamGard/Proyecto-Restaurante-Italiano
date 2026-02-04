# Sistema de Reservas - Ristorante Italini

## Estructura del Proyecto (MVC)

```
ristorante-italini/
│
├── Models/                      # 🎯 ENTIDADES/MODELOS
│   ├── Conexion.php            # Conexión a la base de datos
│   └── Reserva.php             # Clase Reserva (atributos y validaciones)
│
├── Services/                    # 🔧 SERVICIOS (Lógica de negocio)
│   └── ReservaService.php      # Lógica de acceso a datos y consultas SQL
│
├── Controllers/                 # 🎮 CONTROLADORES
│   └── ReservaController.php   # Manejo de peticiones HTTP y respuestas
│
├── Views/                       # 👁️ VISTAS
│   └── admin-reservas.php      # Interfaz de administración
│
├── css/                        # Estilos
│   ├── normalize.css
│   ├── styles.css
│   └── admin-reservas.css
│
├── js/                         # JavaScript
│   ├── app.js                 # JavaScript del sitio principal
│   └── admin-reservas.js      # JavaScript del panel admin
│
├── assets/                     # Imágenes y recursos
├── index.php                  # Página principal
└── api.php                    # 🚪 API REST - PUNTO DE ENTRADA UNIFICADO
```

## Arquitectura en Capas

El proyecto sigue el patrón **MVC con capa de Servicios**:

- **Model (Reserva.php)**: Define la estructura de datos y validaciones
- **Service (ReservaService.php)**: Contiene toda la lógica de negocio y acceso a BD
- **Controller (ReservaController.php)**: Maneja peticiones HTTP y coordina Model/Service
- **View**: Interfaces de usuario (HTML/JS)

Ver [ARQUITECTURA.md](ARQUITECTURA.md) para más detalles.

## Base de Datos

Asegurarse de tener creada la base de datos `ristorante` y la tabla `reservas`:

```sql
CREATE DATABASE IF NOT EXISTS ristorante;
USE ristorante;

CREATE TABLE reservas (
    id INT AUTO_INCREMENT PRIMARY KEY,
    codigo VARCHAR(20) UNIQUE,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    fecha DATE NOT NULL,
    hora TIME NOT NULL,
    personas INT NOT NULL,
    ocasion VARCHAR(50),
    comentarios TEXT,
    estado ENUM('pendiente','confirmada','cancelada') DEFAULT 'pendiente',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

## Configuración

1. **Verificar la conexión a la base de datos** en `Models/Conexion.php`:
   ```php
   $host = "localhost";
   $bd = "ristorante";
   $user = "root";
   $pass = "";
   ```

2. **Iniciar XAMPP**:
   - Apache
   - MySQL

3. **Acceder al proyecto**:
   - Sitio principal: `http://localhost/ristorante-italini/`
   - Panel de administración: `http://localhost/ristorante-italini/Views/admin-reservas.php`

## Funcionalidades Implementadas

### 1. Formulario de Reservas (index.html)
- ✅ Validación de campos en el cliente
- ✅ Envío mediante AJAX a PHP
- ✅ Generación automática de código de reserva
- ✅ Respuesta inmediata al usuario
- ✅ Estado inicial: "pendiente"

### 2. Panel de Administración (Views/admin-reservas.php)
- ✅ Visualización de todas las reservas en tabla
- ✅ Filtros por estado (Todas, Pendientes, Confirmadas, Canceladas)
- ✅ Buscador por nombre, email o código
- ✅ Cambio de estado (Confirmar/Cancelar)
- ✅ Visualización completa de comentarios
- ✅ Estadísticas en tiempo real
- ✅ Diseño acorde a la temática italiana del restaurante

### 3. Backend (PHP - MVC + Services)
- ✅ **Model** (Reserva.php): Entidad con atributos y validaciones
- ✅ **Service** (ReservaService.php): Lógica de negocio y consultas SQL
- ✅ **Controller** (ReservaController.php): Manejo de peticiones HTTP
- ✅ **API REST** (api.php): Punto de entrada unificado con CORS y enrutamiento

## Uso

### Crear una Reserva
1. Acceder a `http://localhost/ristorante-italini/`
2. Ir a la sección de reservas (formulario)
3. Llenar todos los campos requeridos
4. Hacer clic en "Confirmar Reserva"
5. Se mostrará un código de reserva

### Administrar Reservas
1. Acceder a `http://localhost/ristorante-italini/Views/admin-reservas.php`
2. Ver todas las reservas en la tabla
3. Usar filtros para ver por estado
4. Buscar reservas específicas
5. Confirmar o cancelar reservas según sea necesario

## Campos de la Tabla de Reservas

| Campo | Descripción |
|-------|-------------|
| **Código** | Código único generado automáticamente (RES-YYYYMMDD-XXXXXX) |
| **Nombre** | Nombre completo del cliente |
| **Email** | Correo electrónico |
| **Teléfono** | Número de contacto |
| **Fecha y Hora** | Fecha y hora de la reserva concatenadas |
| **Personas** | Cantidad de personas |
| **Ocasión** | Tipo de ocasión (casual, cumpleaños, etc.) |
| **Comentarios** | Comentarios especiales o alergias |
| **Estado** | pendiente / confirmada / cancelada |

## API Endpoints

### Crear Reserva
```
POST /api.php?accion=crear
```
Parámetros: nombre, email, telefono, fecha, hora, personas, ocasion, comentarios

### Listar Reservas
```
GET /api.php?accion=listar
```

### Listar por Estado
```
GET /api.php?accion=listar-por-estado&estado=pendiente
```
Estados válidos: pendiente, confirmada, cancelada

### Obtener Reserva por ID
```
GET /api.php?accion=obtener&id=1
```

### Actualizar Estado
```
POST /api.php?accion=actualizar-estado
```
Parámetros: id, estado

### Actualizar Reserva Completa
```
POST /api.php?accion=actualizar
```
Parámetros: id, nombre, email, telefono, fecha, hora, personas, ocasion, comentarios

### Eliminar Reserva
```
POST /api.php?accion=eliminar
```
Parámetros: id

### Estadísticas
```
GET /api.php?accion=estadisticas
```
Retorna contadores por estado (pendiente, confirmada, cancelada, total)

Ver [API-REFERENCE.md](API-REFERENCE.md) para más detalles.

## Próximas Mejoras (Futuras)
- Editar reserva completa
- Eliminar reserva
- Exportar reservas a Excel/PDF
- Envío de emails de confirmación
- Sistema de autenticación para administradores
- Dashboard con gráficos

## Notas Importantes

1. **Rutas relativas**: Todos los archivos usan rutas relativas desde la carpeta del proyecto
2. **Seguridad**: Los datos se sanitizan con `htmlspecialchars()`
3. **Validación**: Doble validación (cliente y servidor)
4. **Responsive**: El diseño se adapta a dispositivos móviles
5. **Temática**: Los estilos mantienen la identidad visual del restaurante italiano

## Solución de Problemas

### Las reservas no se guardan
- Verificar que MySQL esté corriendo en XAMPP
- Revisar la configuración de `Conexion.php`
- Verificar que la tabla `reservas` exista en la BD

### No se muestra la tabla en admin
- Abrir la consola del navegador (F12) para ver errores
- Verificar que la ruta a `reservas.php` sea correcta
- Revisar permisos de los archivos PHP

### Error 404 al enviar formulario
- Verificar que el archivo `reservas.php` esté en la raíz del proyecto
- Revisar la ruta en `app.js` (línea del fetch)

---

**Desarrollado para Ristorante Italini**
