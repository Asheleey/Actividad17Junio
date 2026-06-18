# Parte 2: Modelado del Ciclo de Vida y Enrutamiento Semántico

## 1. Mapeo Analítico de URLs

En ASP.NET Core, el sistema de enrutamiento convencional utiliza una plantilla predeterminada para interpretar las direcciones URL que recibe la aplicación. A partir de esta estructura, el framework determina automáticamente qué controlador debe atender la solicitud, qué acción debe ejecutar y si existe un parámetro adicional que deba enviarse al método.

La plantilla de enrutamiento utilizada es la siguiente:

```text
{controller=Home}/{action=Index}/{id?}
```

Aplicando esta convención, el resultado del análisis de las siguientes direcciones URL es el siguiente:

| URL Entrante del Cliente | Clase Controladora Buscada por el Framework | Método (Acción) Ejecutado | Parámetro `id` Inyectado |
|---------------------------|---------------------------------------------|---------------------------|--------------------------|
| https://ingenieria.usac.edu.gt/ControlAcademico/Login | ControlAcademicoController | Login | *(Ninguno / Opcional)* |
| https://ingenieria.usac.edu.gt/Estudiante/Historial/20260123 | EstudianteController | Historial | 20260123 |
| https://ingenieria.usac.edu.gt/Asignacion/Detalle/10 | AsignacionController | Detalle | 10 |
| https://ingenieria.usac.edu.gt/Home | HomeController | Index | *(Ninguno / Opcional)* |
