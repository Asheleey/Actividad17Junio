# Actividad de Laboratorio
# Parte 1 - Fundamentación Teórica y Análisis Crítico

## Arquitectura Multi-Nivel (N-Tier) y Patrón MVC en .NET

---

# 1. El Tránsito hacia los Sistemas Distribuidos y Multi-Capa

## La Limitación del Monolito Local

En una arquitectura monolítica todos los componentes de la aplicación (interfaz gráfica, lógica del negocio y base de datos) se ejecutan dentro de una misma computadora física.

Aunque este modelo es sencillo para aplicaciones pequeñas, presenta varios problemas cuando el sistema comienza a crecer.

### Problemas principales

- Existe un único punto de fallo: si la computadora deja de funcionar, todo el sistema queda fuera de servicio.
- La capacidad de procesamiento depende únicamente del hardware de esa máquina.
- La escalabilidad es muy limitada, ya que solo puede mejorarse aumentando los recursos del mismo equipo (CPU, memoria o almacenamiento).
- Es difícil permitir que varios usuarios trabajen simultáneamente con un buen rendimiento.
- La sincronización de datos entre diferentes equipos resulta complicada cuando existen varias copias de la aplicación.
- El mantenimiento requiere detener completamente el sistema.

### Ejemplo

Supongamos un sistema de control académico instalado únicamente en la computadora del administrador.

- Los estudiantes no pueden acceder desde otros lugares.
- Si el disco duro falla, toda la información queda inaccesible.
- Si cien usuarios intentan utilizar el sistema al mismo tiempo, el rendimiento disminuye considerablemente.

Por estas razones surgieron las arquitecturas distribuidas y posteriormente la arquitectura Multi-Tier.

---

# Distinción Crítica (Layers vs. Tiers)

Aunque muchas veces se utilizan como sinónimos, **Layers** y **Tiers** representan conceptos diferentes.

## Layers (Capas Lógicas)

Las capas representan una separación del código fuente según sus responsabilidades.

No dependen del hardware donde se ejecuta la aplicación.

Su objetivo principal es organizar el software para facilitar:

- mantenimiento
- reutilización
- pruebas
- escalabilidad del código

Ejemplos:

- Capa de Presentación
- Capa de Negocio
- Capa de Acceso a Datos

---

## Tiers (Niveles Físicos)

Los niveles representan la distribución física de la aplicación.

Cada nivel puede ejecutarse en un servidor diferente conectado mediante una red.

Su objetivo es mejorar:

- rendimiento
- seguridad
- disponibilidad
- escalabilidad física

Ejemplo:

Servidor 1
- Aplicación Web

Servidor 2
- API de Negocio

Servidor 3
- Base de Datos SQL Server

---

## Diferencia principal

| Layers | Tiers |
|---------|--------|
| Separación lógica | Separación física |
| Organizan el código | Organizan la infraestructura |
| Mejoran mantenimiento | Mejoran rendimiento y escalabilidad |
| Existen dentro del software | Existen en diferentes servidores |

---

# Responsabilidades en la Arquitectura de 3 Niveles

## Nivel 1 - Presentation Tier

También llamado capa de presentación.

### Responsabilidad

Es la parte con la que interactúa directamente el usuario.

Se encarga de:

- mostrar información
- recibir datos del usuario
- enviar solicitudes al servidor
- mostrar los resultados obtenidos

No debe contener reglas del negocio.

### Tecnologías comunes

- ASP.NET Core MVC
- Razor Pages
- HTML
- CSS
- JavaScript
- Bootstrap

---

## Nivel 2 - Application Tier

También conocido como capa de negocio.

### Responsabilidad

Implementa todas las reglas del sistema.

Entre sus funciones se encuentran:

- validar información
- calcular resultados
- ejecutar procesos
- controlar permisos
- comunicarse con la base de datos

Esta capa actúa como intermediaria entre la interfaz y el almacenamiento.

### Tecnologías comunes

- ASP.NET Core
- C#
- Servicios Web
- APIs REST
- .NET

---

## Nivel 3 - Data Tier

Corresponde a la capa de almacenamiento.

### Responsabilidad

Se encarga de administrar toda la información del sistema.

Realiza operaciones como:

- insertar registros
- consultar información
- actualizar datos
- eliminar registros
- garantizar integridad de los datos

No contiene lógica del negocio.

### Tecnologías comunes

- SQL Server
- PostgreSQL
- MySQL
- Oracle
- SQLite

---

# Seguridad Perimetral

Una base de datos nunca debe exponerse directamente a Internet.

### Riesgos

Si el puerto de la base de datos es público, un atacante podría:

- intentar acceder mediante fuerza bruta
- robar información confidencial
- modificar registros
- eliminar datos
- instalar software malicioso
- comprometer todo el sistema

Además, la base de datos no está diseñada para atender solicitudes provenientes directamente de usuarios externos.

### Buena práctica

La arquitectura recomendada consiste en:

Usuario

↓

Servidor Web

↓

Servidor de Aplicaciones

↓

Base de Datos

La base de datos únicamente acepta conexiones provenientes del servidor de aplicaciones.

Esto permite:

- mayor seguridad
- autenticación
- autorización
- validación de datos
- monitoreo de accesos
- menor exposición a ataques

---

# 2. Desacoplamiento Lógico con el Patrón MVC

## La Crisis del Código Espagueti

El término "Código Espagueti" describe aplicaciones donde todo el código está mezclado dentro de un mismo archivo.

Por ejemplo:

- código HTML
- consultas SQL
- reglas matemáticas
- validaciones
- estilos visuales

Todo junto.

### Problemas

- Muy difícil de leer.
- Difícil de modificar.
- Mayor probabilidad de errores.
- Las pruebas unitarias casi no pueden realizarse.
- Cambiar una parte puede afectar muchas otras.
- El mantenimiento resulta costoso.

Por ello surgió el patrón MVC.

---

# Separación de Preocupaciones (Separation of Concerns - SoC)

El principio SoC indica que cada componente debe tener una única responsabilidad.

El patrón MVC implementa este principio dividiendo la aplicación en tres componentes.

---

## Modelo (Model)

Representa los datos y las reglas del negocio.

Es responsable de:

- almacenar información
- validar datos
- representar entidades
- comunicarse con la base de datos

El Modelo nunca debe conocer cómo se muestran los datos al usuario.

Esto permite reutilizar el mismo modelo desde:

- una aplicación web
- una aplicación móvil
- una API
- una aplicación de escritorio

---

## Vista (View)

La Vista representa únicamente la interfaz gráfica.

Su función consiste en mostrar información al usuario.

Debe contener únicamente:

- HTML
- Razor
- CSS
- pequeñas expresiones para mostrar datos

Está estrictamente prohibido colocar dentro de la Vista:

- consultas SQL
- cálculos matemáticos
- reglas del negocio
- acceso directo a la base de datos

Por esta razón se considera una entidad pasiva.

---

## Controlador (Controller)

El Controlador funciona como intermediario entre la Vista y el Modelo.

Su trabajo consiste en:

- recibir las solicitudes HTTP
- interpretar la petición
- llamar al Modelo
- obtener los resultados
- enviar la información a la Vista correspondiente

Se le conoce como el "director de orquesta" porque coordina la comunicación entre todos los componentes.

---

# Métricas de Ingeniería de Software

## Alta Cohesión

La cohesión mide qué tan relacionadas están las responsabilidades de una clase.

En MVC:

- cada componente tiene una única responsabilidad.
- el Modelo administra datos.
- la Vista muestra información.
- el Controlador coordina el flujo.

Esto produce una alta cohesión.

---

## Bajo Acoplamiento

El acoplamiento mide cuánto depende un componente de otro.

MVC reduce estas dependencias.

Por ejemplo:

- cambiar el diseño de la Vista no obliga a modificar el Modelo.
- cambiar la base de datos no requiere modificar la Vista.
- el Controlador únicamente coordina la comunicación.

Gracias al bajo acoplamiento es posible:

- mantener el sistema fácilmente.
- agregar nuevas funciones.
- realizar pruebas unitarias.
- reutilizar componentes.
- trabajar varios desarrolladores simultáneamente.

---

# Conclusión

La arquitectura Multi-Tier permite distribuir físicamente una aplicación para mejorar su rendimiento, disponibilidad y seguridad, mientras que el patrón MVC organiza internamente el código mediante la separación de responsabilidades.

La combinación de ambas arquitecturas permite desarrollar aplicaciones escalables, fáciles de mantener, seguras y alineadas con las buenas prácticas de ingeniería de software utilizadas actualmente en ASP.NET Core y .NET.

---

# Referencias Bibliográficas

- Facultad de Ingeniería, Universidad de San Carlos de Guatemala. (2026). **Sesión 11: Modelado Base y Arquitecturas de Despliegue. Evolución de Sistemas Distribuidos, Fundamentos del Modelo Cliente-Servidor y Diseño Físico Multi-Capas (N-Tier).** Laboratorio de Introducción a la Programación y Computación 2.

- Facultad de Ingeniería, Universidad de San Carlos de Guatemala. (2026). **Sesión 12: Arquitectura y Componentes del Patrón MVC. Desacoplamiento Lógico de Software, Ciclo de Vida de las Peticiones y Enrutamiento en Aplicaciones Interactivas Modernas.** Laboratorio de Introducción a la Programación y Computación 2.

- Microsoft Learn. **ASP.NET Core MVC Overview.**

- Microsoft Learn. **ASP.NET Core Architecture.**
