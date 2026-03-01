# 🍲 MacroForge Nexus

![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.2-brightgreen.svg)
![Java](https://img.shields.io/badge/Java-21-blue.svg)
![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)
![Testing](https://img.shields.io/badge/Testing-JUnit%20%7C%20Integration-success.svg)

## 📖 Descripción del Proyecto

**MacroForge Nexus** es el sistema centralizado de gestión de órdenes y monitoreo para la cocina automatizada de un campus universitario de alto rendimiento. 

El proyecto nace de la necesidad de alimentar a dos grupos demográficos muy exigentes dentro de la institución: atletas enfocados en ganar masa muscular (fase de volumen) con presupuestos limitados, y equipos de programación competitiva que requieren combustible de alta calidad para sus intensas sesiones de entrenamiento. 

El sistema gestiona todo el ciclo de vida de las órdenes alimenticias respetando las estrictas (y algo inusuales) normativas de salud del campus:
* Prohibición absoluta de huevos fritos o duros en las instalaciones; toda proteína a base de huevo se procesa de forma segura mediante máquinas especializadas como waffles u omelettes.
* Gestión prioritaria del postre estrella del campus, catalogado internamente como el "elixir" (la combinación milimétrica de queso y dulce de batata).

## 🎯 El Problema a Resolver

A nivel técnico, la cocina central enfrenta picos extremos de demanda durante los horarios de almuerzo, generando cuellos de botella tanto en la preparación física como en la infraestructura de software. 

**MacroForge Nexus** resuelve las siguientes problemáticas críticas:
1.  **Concurrencia y Sobrecarga:** El servidor web debe ser capaz de soportar a cientos de estudiantes enviando órdenes simultáneas sin degradar los tiempos de respuesta.
2.  **Integridad de Estados:** Una orden de comida no puede ser cancelada si ya está en la plancha, ni entregada si no ha sido pagada. Se requiere un control de estados riguroso y a prueba de fallos.
3.  **Lógica de Negocio Dinámica:** Los cálculos de macronutrientes y precios varían drásticamente dependiendo del perfil del estudiante y la estrategia de alimentación seleccionada.
4.  **Trazabilidad y Monitoreo:** El equipo de operaciones necesita visibilidad en tiempo real del rendimiento del servidor y un registro histórico segmentado por severidad para auditar fallos.

## 🛠️ Tecnologías Utilizadas

* **Backend:** Java, Spring Boot, Spring Web.
* **Persistencia:** Spring Data JPA, MySQL (Motor de base de datos relacional).
* **Validaciones:** Jakarta Bean Validation.
* **Frontend (SSR):** Thymeleaf, HTML5, Bootstrap (Interfaces responsivas y funcionales).
* **Herramientas de Desarrollo:** Spring Boot DevTools.
* **Infraestructura y Despliegue:** Docker, Docker Compose.
* **Servidor Web Embebido:** Apache Tomcat.
* **Testing:** JUnit 5 (Caja blanca), Mockito, Spring Boot Test (Caja negra/Integración), herramientas de Load Testing (JMeter/Gatling).

## 🧩 Patrones de Diseño Aplicados

Para asegurar que el código sea escalable, mantenible y siga los principios SOLID, la arquitectura del software implementa los siguientes patrones de diseño:

* **State (Máquina de Estados)**
* **Strategy**
* **Factory**
* **Singleton**
* **DTO (Data Transfer Object)**
* **MVC (Model-View-Controller)**
* **Builder**
* **Observer**

## ⚙️ Arquitectura y Buenas Prácticas de Ingeniería

Este proyecto no es solo un CRUD, sino una aplicación lista para producción que simula un entorno empresarial real:

* **Gestión de Logs:** Implementación de rotación diaria de logs, segregando la información en distintos archivos según su nivel de gravedad (`INFO`, `WARN`, `ERROR`) para facilitar el análisis de incidentes.
* **Análisis de Rendimiento:** Monitoreo activo del servidor web Apache Tomcat, analizando la gestión de hilos (threads) y el consumo de memoria bajo estrés.
* **Testing Exhaustivo:** * *Unit Testing (Caja Blanca):* Pruebas aisladas de la lógica de negocio y los motores de reglas.
    * *Integration Testing (Caja Negra):* Verificación del flujo completo desde los controladores HTTP hasta la persistencia en MySQL.
    * *Load/Stress Testing:* Simulaciones de alta demanda (ej. 500 peticiones concurrentes) para validar la resiliencia del servidor.