Project: MacroForge Nexus


Contexto del Negocio

Una prestigiosa universidad ha decidido revolucionar la forma en que se alimentan sus estudiantes de alto rendimiento. Han notado que dos grupos en particular necesitan dietas muy específicas: los atletas que buscan ganar masa muscular ("hacer volumen") con un presupuesto ajustado, y los equipos de programación competitiva que necesitan energía limpia para sus largas jornadas de entrenamiento de cara al ICPC.

Para solucionar esto, están construyendo una cocina automatizada y centralizada en el campus. La política de salud del campus es muy estricta y algo excéntrica: están terminantemente prohibidos los huevos duros o fritos; toda proteína a base de huevo debe procesarse estrictamente como omelette o waffle. Además, el postre estrella, catalogado en el sistema como un "elixir" nutricional, es una combinación exacta de queso y dulce de batata.

Tu misión es desarrollar el sistema central que gestionará las órdenes, la preparación de estas comidas específicas y el monitoreo del servidor que soportará a miles de estudiantes hambrientos haciendo peticiones al mismo tiempo.


Requerimientos de Arquitectura y Patrones de Diseño

El núcleo de MacroForge Nexus no debe ser un simple CRUD. Deberás aplicar los siguientes patrones para mantener el código limpio y escalable:

-State (Máquina de Estados): El ciclo de vida de una orden de comida es complejo. Una orden pasa por los estados: Creada -> Validando Macros -> En Cocina (Preparando) -> Lista para Retirar -> Entregada. Si un estudiante se queda sin saldo, pasa a Cancelada. Cada estado tiene reglas estrictas (ej. no puedes cancelar una orden que ya está En Cocina).
-Strategy: Los estudiantes tienen diferentes planes de precios y cálculos de calorías. Debes implementar un motor de estrategias para calcular el costo final y el desglose de macronutrientes dependiendo de si el estudiante tiene un perfil "Budget Bulk" (volumen económico) o "Premium Athlete".
-Factory: La creación de los platillos es compleja. Necesitas una fábrica que, dependiendo de la petición del estudiante, instancie los objetos correctos (ej. OmeletteFactory o WaffleFactory) asegurando que nunca se cree por error un huevo frito.
-Singleton: La conexión con el hardware de la cocina automatizada (simulada) debe ser un único punto de acceso global para evitar sobrecargar los comandos de los robots de cocina.
-DTO (Data Transfer Object): La información que viaja desde la base de datos (Entidades JPA) hasta la vista (Thymeleaf) debe estar completamente separada. Los controladores solo devolverán y recibirán DTOs para evitar exponer la estructura interna de la base de datos y prevenir vulnerabilidades.
-MVC (Model-View-Controller): Utilizarás esta arquitectura por defecto con el ecosistema Spring, separando claramente las responsabilidades entre tus Controladores (Web), tu lógica de Negocio (Servicios/Modelos) y tus Vistas.
Patrones a estudiar para aplicarlos:
-Builder: Ideal para construir objetos muy complejos paso a paso. Perfecto para que un estudiante "arme" su platillo personalizado con decenas de ingredientes opcionales sin tener constructores gigantescos.
-Observer (o Pub/Sub): Muy útil para emitir eventos en el sistema. Por ejemplo, cuando una orden cambia al estado Lista para Retirar, un "observador" reacciona y genera un registro o envía una notificación sin acoplar la lógica de la orden con la de notificaciones.


Stack Tecnológico y Buenas Prácticas

-Backend: Spring Boot con Spring Web. Toda la configuración debe apoyarse en Spring Boot DevTools para agilizar tu desarrollo local.
-Persistencia: Spring Data JPA conectado a una base de datos MySQL. Deberás diseñar un modelo relacional robusto (Usuarios, Órdenes, Platillos, Transacciones).
-Validaciones: Uso exhaustivo de Jakarta Bean Validation. No se puede procesar una orden con calorías negativas, ni nombres vacíos, ni presupuestos menores a cero. Todo debe validarse antes de tocar la lógica de negocio.
-Frontend (UI): Renderizado del lado del servidor usando Thymeleaf. Como indicaste, no hace falta que sea una obra de arte, así que integrarás Bootstrap para tener vistas limpias, responsivas y funcionales rápidamente. Aquí los estudiantes verán el menú, armarán sus platillos y verán el estado de su orden en tiempo real.


Requerimientos de Operaciones, Monitoreo y Testing

1. Gestión de Logs Avanzada
No basta con imprimir en consola. Debes configurar el sistema de logging (ej. Logback, integrado en Spring) para que:

-Se generen archivos de log rotativos por día.
-Se dividan por niveles de gravedad: Un archivo para INFO (ej. "Orden #123 creada"), otro exclusivo para WARN y ERROR (ej. "Fallo en la conexión con la base de datos" o "Intento de inyección de estado inválido").

2. Análisis de Rendimiento de Apache Tomcat

Al ser la hora del almuerzo, el servidor recibe miles de peticiones concurrentes. Deberás configurar la aplicación (puedes usar Spring Boot Actuator) para exponer métricas del servidor web Tomcat embebido. El objetivo es que puedas analizar el uso de hilos (threads) de Tomcat, el consumo de memoria y los tiempos de respuesta de los endpoints bajo estrés.

3. Testing Completo y Automatizado
Debes entregar una suite de pruebas robusta:

-Caja Blanca (Unit Testing): Pruebas exhaustivas sobre tu lógica de negocio (State, Strategy, Factory) aisladas de la base de datos usando JUnit y mocks (ej. Mockito). Debes probar cada bifurcación del código.
-Caja Negra (Integration Testing): Pruebas que levanten el contexto de Spring y ataquen los endpoints HTTP simulando a un usuario real, verificando que la base de datos se actualice correctamente.
-Pruebas de Sobrecarga (Load Testing): Deberás incluir scripts (puedes usar herramientas como JMeter o Gatling) para simular 500 estudiantes pidiendo su "elixir" de queso y dulce de batata al mismo tiempo, y analizar cómo responden el Tomcat y tus logs.
-Automatización: La suite debe estar configurada (por ejemplo, vía Maven o Gradle) para que los tests se puedan correr bajo demanda con un simple comando, o integrarse para correr automáticamente antes de cualquier despliegue.

4. Contenerización con Docker
-Todo el proyecto debe estar dockerizado.
-Deberás proveer un archivo docker-compose.yml que levante de un solo golpe: La aplicación Spring Boot, la base de datos MySQL, y opcionalmente un contenedor para centralizar los logs o ver las métricas del servidor.