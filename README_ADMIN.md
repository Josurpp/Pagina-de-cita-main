# Sistema de Administración - Documentación

## Descripción General
Se ha implementado un sistema completo de login y panel de administración para la página de citas médicas. El administrador puede ver, filtrar, actualizar el estado y eliminar citas de todos los pacientes.

## Nuevos Archivos Creados

### 1. `admin-login.html`
Página de autenticación del administrador.

**Características:**
- Formulario de login con campos Usuario y Contraseña
- Credenciales hardcodeadas: `usuario: admin`, `contraseña: admin123`
- Validación de credenciales en el cliente
- Mensajes de error y éxito personalizados
- Diseño responsivo con gradiente azul
- Redirección automática al panel si las credenciales son correctas
- Redirección al login si intenta acceder al panel sin autenticarse

**localStorage utilizado:**
- `adminLogueado`: Se establece en "si" cuando el login es exitoso

### 2. `admin-panel.html`
Panel de administración de citas.

**Características:**
- **Header personalizado:** Identifica el panel como "Sistema de Citas Médicas - Admin"
- **Botón de Cerrar Sesión:** Permite logout con confirmación
- **Estadísticas en Tiempo Real:**
  - Citas Pendientes (naranja)
  - Citas Atendidas (verde)
  - Total de Citas (azul)

- **Sistema de Búsqueda y Filtros:**
  - Buscar por nombre de paciente
  - Filtrar por especialidad
  - Filtrar por estado (Pendiente, Atendida, Cancelada)
  - Botones Buscar y Limpiar filtros

- **Tabla de Citas:**
  - Columnas: Paciente, Especialidad, Médico, Fecha, Hora, Estado, Acciones
  - Estados mostrados con badges de color
  - Botones de acción:
    - ✓ Marcar como Atendida (verde, deshabilitado si ya está atendida)
    - ✕ Eliminar (rojo, siempre activo)

- **Protección de Acceso:**
  - Redirige a `admin-login.html` si no hay sesión de admin
  - Sincroniza cambios entre pestañas (event listener para cambios en localStorage)

## Archivos Modificados

### 1. `index.html`
Se agregó:
- Enlace "Panel Admin" en el header (visible siempre)
- Script actualizado para detectar estado de `adminLogueado`

### 2. `style.css`
Se agregaron:
- Estilos para `.btn-admin` con color rojo oscuro
- Efecto hover para el botón de admin
- Mantenida consistencia con el diseño existente

## Estructura de Datos en localStorage

```javascript
// Citas de pacientes
{
  "citas": [
    {
      "paciente": "Juan Pérez",
      "especialidad": "Cardiología",
      "medico": "Dr. López",
      "fecha": "2026-06-15",
      "hora": "10:30",
      "estado": "Pendiente" | "Atendida" | "Cancelada"
    },
    // ... más citas
  ],
  
  // Estado de sesión admin
  "adminLogueado": "si" | "no"
}
```

## Flujo de Uso

### Login del Administrador
1. El usuario hace clic en "Panel Admin" en el header
2. Se abre `admin-login.html`
3. Ingresa credenciales: `admin` / `admin123`
4. Sistema valida las credenciales
5. Si son correctas:
   - Se guarda `adminLogueado: "si"` en localStorage
   - Se redirige a `admin-panel.html`
6. Si son incorrectas:
   - Se muestra mensaje de error en rojo
   - Se limpian los campos

### Uso del Panel de Administración
1. El administrador ve todas las citas en una tabla
2. Las estadísticas se actualizan en tiempo real
3. Puede:
   - **Buscar citas** por nombre de paciente
   - **Filtrar** por especialidad y estado
   - **Cambiar estado** a "Atendida" (si está en pendiente)
   - **Eliminar citas** (con confirmación)
4. Los cambios se guardan en localStorage
5. Para cerrar sesión:
   - Hace clic en "Cerrar Sesión"
   - Confirma en el diálogo
   - Se elimina `adminLogueado` de localStorage
   - Se redirige a `index.html`

## Seguridad y Protección

⚠️ **Nota importante:** Este sistema es para un sitio estático y usa localStorage. Las credenciales están visibles en el código fuente. Para un sistema de producción:
- Usar autenticación en servidor
- Transmitir credenciales de forma segura (HTTPS)
- Implementar tokens de sesión con expiración
- Usar hash para las contraseñas

## Características de Diseño

- **Diseño Responsivo:** Se adapta a diferentes tamaños de pantalla
- **Accesibilidad:** Incluye focus outlines y navegación por teclado
- **Consistencia Visual:** Usa los mismos colores (#1e88e5 azul, #e53935 rojo)
- **Feedback de Usuario:** Mensajes de confirmación y error claros

## Compatibilidad

- Navegadores modernos (Chrome, Firefox, Safari, Edge)
- JavaScript vanilla, sin dependencias externas
- CSS3 con media queries para responsividad

## Testing Manual Realizado

✅ Login con credenciales correctas funciona  
✅ Login con credenciales incorrectas muestra error  
✅ Panel de admin se abre después del login exitoso  
✅ Protección de acceso: redirige a login si se accede sin autenticarse  
✅ Logout funciona y redirige a inicio  
✅ Tabla de citas se muestra vacía cuando no hay datos  
✅ Estadísticas se actualizan en tiempo real  
✅ Filtros y búsqueda son funcionales  

## Próximas Mejoras Opcionales

- Agregar validación de fecha/hora para nuevas citas
- Implementar paginación en la tabla si hay muchas citas
- Agregar exportación de citas a PDF o CSV
- Implementar más roles de usuario (enfermero, recepcionista)
- Agregar historial de cambios de citas
- Notificaciones por correo para citas atendidas
