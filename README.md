# Sistema de Gestión de Turnos Digitales (Tunomático)

## Descripción General del Sistema

Este proyecto corresponde al modelado completo de un Sistema de Gestión de Turnos Digitales (Tunomático), diseñado para optimizar la atención al cliente mediante la automatización de la asignación y seguimiento de turnos en entornos como hospitales, bancos o instituciones públicas.

El sistema considera los siguientes aspectos clave:

- Generación automática de números de turno con categorización por tipo de servicio
- Visualización en tiempo real de turnos mediante pantallas digitales
- Notificaciones a clientes cuando su turno está próximo
- Gestión de múltiples estaciones de atención
- Reportes estadísticos de tiempos de espera y atención
- Integración con sistemas externos de gestión de colas

## Objetivos del Modelado

- Implementar una solución completa desde el análisis funcional hasta el despliegue físico
- Aplicar patrones de diseño para garantizar escalabilidad y mantenibilidad
- Diseñar una arquitectura modular que permita futuras extensiones

---

## 1. Diagrama de Casos de Uso UML
![image](https://github.com/user-attachments/assets/6804f90b-31e8-42e0-b494-def35907457f)

### Sistema Tunomático

- Generar Turno
- Llamar Siguiente Turno
- Reasignar Turno
- Cancelar Turno
- Configurar Servicios
- Visualizar Turnos en Pantalla
- Generar Reportes Estadísticos
- Notificar Cliente

### Relaciones Especiales

- `<<extend>>` Notificar Cliente al Generar Turno
- `<<include>>` Generar Reportes Estadísticos al Llamar Turno
- `<<extend>>` Reasignar Turno al Llamar Siguiente Turno

### Descripción general del Diagrama de Casos de Uso

**Actores identificados:**

- Cliente: Solicita turnos, recibe notificaciones
- Operador: Gestiona la llamada de turnos, realiza reasignaciones
- Administrador: Configura servicios, genera reportes
- Pantalla Digital: Muestra información de turnos en tiempo real
- Sistema de Notificaciones: Envía alertas a clientes

**Casos de uso destacados y relaciones aplicadas:**

- **Generar Turno**
  - `<<extend>>` Notificar Cliente: envía confirmación al cliente de forma opcional
- **Llamar Siguiente Turno**
  - `<<include>>` Generar Reportes Estadísticos: registra automáticamente tiempos de espera
- **Reasignar Turno**
  - `<<extend>>` de Llamar Siguiente Turno: permite reasignación cuando el turno no es atendido
- **Configurar Servicios**
  - Relación directa con Pantalla Digital para actualizar servicios mostrados

**Justificación de las relaciones:**

- Se aplicó `<<include>>` para acciones obligatorias como el registro estadístico
- Se usó `<<extend>>` para funcionalidades opcionales como notificaciones
- Las relaciones reflejan el flujo real de un sistema de gestión de turnos

---

## 2. Diagrama de Clases UML con Patrones Aplicados
![image](https://github.com/user-attachments/assets/b43ecf84-a708-41f7-8b45-1b3d8b1156e6)

### Justificación Arquitectónica y Patrones Aplicados

#### 1. Singleton (GestorTurnos)

**Justificación:**

Garantiza una única instancia del gestor de turnos para mantener consistencia en la numeración y evitar conflictos.

**Implementación:**

- Atributo estático `instancia`
- Constructor privado
- Método público `getInstancia()`

#### 2. Prototype (PlantillaTurno)

**Justificación:**

Permite clonar turnos preconfigurados para diferentes tipos de servicio, agilizando la creación de nuevos turnos.

**Implementación:**

- Interfaz `ClonableTurno` con método `clonar()`
- Clases concretas para cada tipo de turno

#### 3. Adapter (AdaptadorPantalla)

**Justificación:**

Adapta la interfaz del sistema a diferentes tipos de pantallas digitales (LED, LCD, etc.).

**Implementación:**

- Interfaz `IPantalla` con método `mostrarTurno()`
- Clases adaptadoras para cada tipo de pantalla

#### 4. Bridge (Notificador)

**Justificación:**

Separa la lógica de notificación de los diferentes canales (SMS, Email, App).

**Implementación:**

- Clase abstracta `Notificador`
- Implementaciones concretas para cada canal
- Interfaz `NotificadorImpl`

---

## 3. Diagrama de Implementación UML
![image](https://github.com/user-attachments/assets/f98db4fd-1c6f-4c88-8733-5c804a086ceb)

### Arquitectura Física:

- **Servidor Central:** Hostea el GestorTurnos (Singleton) y la base de datos
- **Terminales de Operadores:** Clientes ligeros que acceden al servidor central
- **Pantallas Digitales:** Nodos específicos para visualización
- **Servicio de Notificaciones:** Componente independiente para enviar alertas

### Decisiones Técnicas:

- El Singleton se implementa en el servidor central para garantizar consistencia
- Los adaptadores de pantalla permiten conectar diferentes tecnologías de visualización
- El patrón Bridge en el notificador facilita añadir nuevos canales de comunicación

---

## Reflexiones Finales del Modelado

- `Singleton` demostró ser esencial para mantener la integridad en la generación de turnos
- `Prototype` optimizó la creación de turnos recurrentes con configuraciones similares
- `Adapter` permitió una integración flexible con diferentes tecnologías de pantalla
- `Bridge` hizo el sistema extensible para nuevos canales de notificación

### El modelado logró:

- Trazabilidad clara desde requisitos hasta implementación
- Modularidad que facilita mantenimiento y extensiones
- Aplicación práctica de patrones con justificación arquitectónica sólida

---

## Nota

Este modelo arquitectónico sirve como base para la implementación técnica, mostrando cómo los patrones de diseño resuelven problemas específicos en un sistema de gestión de turnos digital.
