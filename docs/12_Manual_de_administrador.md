# 12 — Manual de Administrador

## 12.1 Acceso

El administrador inicia sesión con su cuenta de Google autorizada. El sistema valida que el email exista en la tabla `users` con `role = 'admin'` y `is_active = true`.

## 12.2 Dashboard del administrador

Al iniciar sesión, el administrador accede al Dashboard Principal con accesos a:

- Gestión de estudiantes.
- Gestión de docentes.
- Gestión de materias.
- Gestión de asignaciones.
- Registro de notas.
- Registro de asistencia.

## 12.3 Gestión de estudiantes

### Registrar estudiante

1. Ir a "Estudiantes" → "Registrar".
2. Completar el formulario con:
   - Nombres y apellidos (mayúscula inicial, mínimo 3 caracteres).
   - CI (6–8 dígitos, único).
   - RUDE (6–20 dígitos, único).
   - Fecha de nacimiento.
   - Curso y paralelo.
   - Datos del tutor: nombres, apellidos, email y teléfono (8 dígitos, empieza con 6 o 7).
3. Hacer clic en "Registrar".
4. Si hay errores de validación, se muestran mensajes específicos por campo.
5. Si el registro es exitoso, el estudiante aparece en la lista.

### Editar estudiante

1. Ir a "Estudiantes" → lista.
2. Hacer clic en el estudiante a editar.
3. Modificar los campos necesarios.
4. Hacer clic en "Guardar".

### Listar estudiantes

1. Ir a "Estudiantes" → "Lista".
2. Se muestra la lista completa con nombres, CI, RUDE, curso y paralelo.

## 12.4 Gestión de docentes

### Registrar docente

1. Ir a "Docentes" → "Registrar".
2. Completar el formulario con:
   - Nombres y apellidos.
   - CI (único).
   - Email (único).
   - Teléfono (8 dígitos, empieza con 6 o 7).
   - Fecha de nacimiento.
   - Especialidad.
   - Fecha de contratación.
3. Hacer clic en "Registrar".

### Editar docente

1. Ir a "Docentes" → lista.
2. Hacer clic en el docente a editar.
3. Modificar los campos necesarios.
4. Hacer clic en "Guardar".

## 12.5 Gestión de materias

1. Ir a "Materias".
2. Para crear: hacer clic en "Nueva materia", completar nombre y nivel (`inicial`, `primaria`, `secundaria`), guardar.
3. Para editar: hacer clic en la materia, modificar y guardar.
4. Para eliminar: hacer clic en "Eliminar" y confirmar.

## 12.6 Gestión de asignaciones

1. Ir a "Asignaciones".
2. Para crear: seleccionar docente, materia, curso y paralelo; hacer clic en "Asignar".
3. Para eliminar: hacer clic en "Eliminar" y confirmar.

## 12.7 Registro de notas

1. Ir a "Notas" → "Registrar".
2. Seleccionar estudiante, materia, trimestre.
3. Ingresar los cinco componentes: SER, SABER, HACER, DECIDIR, Autoevaluación.
4. El sistema calcula el total automáticamente.
5. Hacer clic en "Guardar".

## 12.8 Registro de asistencia

1. Ir a "Asistencia" → "Registrar".
2. Seleccionar estudiante, materia, fecha.
3. Seleccionar estado: `presente`, `tarde` o `falta`.
4. Hacer clic en "Guardar".

## 12.9 Cierre de sesión

- La sesión se cierra automáticamente tras 10 minutos de inactividad.
- También se puede cerrar manualmente desde el botón "Cerrar sesión".