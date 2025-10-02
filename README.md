# Sistema Distribuido de Detección y Alerta Temprana de Plagas

Este proyecto presenta una solución **escalable y de bajo coste** para el **monitoreo y alerta temprana de plagas**. Implementa un sistema de detección distribuido que utiliza **sensores ultrasónicos** para medir la proximidad y activar mecanismos de alerta (LEDs y buzzers). Se ha diseñado para ofrecer **gestión en tiempo real y control remoto** mediante los protocolos de comunicación **HTTP (REST)** y **MQTT**.

**Desarrollado por:** Josemaría Solís de la Osa y equipo.

---

## Funcionalidades Clave

- **Detección de Proximidad de Plagas**:
  El **sensor ultrasónico HC-SR04** realiza mediciones de distancia de forma continua. Al detectar un objeto (posible plaga) a una distancia **inferior al umbral configurado (20 cm)**, se activa inmediatamente un **LED** y un **buzzer** como actuadores de alerta local.

- **Control Remoto de Actuadores**:
  Los actuadores (LED y buzzer) pueden ser **encendidos o apagados manualmente** a distancia a través de comandos específicos.

- **Arquitectura Distribuida y Adaptable**:
  Cada módulo se basa en el **microcontrolador ESP32**, el cual se conecta a la red WiFi. La comunicación mediante protocolos estándar (REST y MQTT) asegura una fácil integración en diversos **escenarios como agricultura de precisión, invernaderos o almacenes**.

- **Robustez y Conectividad**:
  El sistema está programado para gestionar la pérdida de conexión mediante la **reconexión automática** tanto a la red WiFi como al broker MQTT, garantizando la fiabilidad operativa.

---

## Componentes Tecnológicos

- **Microcontrolador Principal**: ESP32
- **Sensor de Detección**: Ultrasónico HC-SR04
- **Dispositivos de Alerta**: LED y buzzer
- **Protocolos de Comunicación**: MQTT (para datos ligeros en tiempo real) y HTTP (REST)
- **Infraestructura de Datos**: API REST para el backend (almacenamiento y gestión)
- **Servidor de Mensajería**: Broker MQTT
- **Lenguajes de Programación**: C++, Java y SQL.

---

## Instrucciones de Despliegue

1. **Montaje Físico**: Instalar los sensores ultrasónicos, buzzers y LEDs en las zonas de monitoreo. Cada LED debe estar asociado a su sensor para identificación.
2. **Configuración del Umbral**: Ajustar el límite de distancia para la detección de la plaga.
3. **Conexión a la Red**: Proveer las credenciales (SSID y contraseña) para la conexión WiFi de la placa.
4. **Broker MQTT**: Instalar y configurar el servicio de broker MQTT.
5. **Servidor Backend**: Desplegar el servidor con la API REST encargada del almacenamiento y la consulta de datos.
6. **Suscripción de Control**: Suscribir el sistema al *topic* MQTT para recibir comandos de control remoto. Los mensajes `"ON"` (control automático por sensor) y `"OFF"` (desactivación manual) permiten la gestión remota.

---

## Estructura del Código

El proyecto se divide en dos módulos principales: el **Backend-API** y el **Firmware**.

---

### Backend-API

El **módulo Backend-API** es el núcleo de gestión de datos, diseñado para centralizar la información de todos los nodos (sensores y actuadores):

- **Endpoints de Sensores**:
  - `GET /api/sensor` → Consulta de todos los sensores registrados.
  - `GET /api/sensor/:idSensor` → Recupera datos de un sensor específico.
  - `GET /api/sensor/last` → Obtiene la lectura más reciente.
  - `POST /api/sensor_values` → Envía y registra una nueva lectura de distancia.

- **Endpoints de Actuadores**:
  - `GET /api/actuador` → Listado de todos los actuadores del sistema.
  - `GET /api/actuador/:idActuador` → Detalle de un actuador específico.
  - `POST /api/actuator_states` → Actualiza el estado actual del actuador.

- **Persistencia de Datos**: Almacenamiento histórico de lecturas y estados para análisis de auditoría y tendencias.

---

### Firmware (ESP32)

El **Firmware** reside en la placa **ESP32** y gestiona la interacción directa con el hardware y la red:

- **Adquisición de Datos**: Tarea constante de lectura de distancia a través del sensor ultrasónico.
- **Control Local**: Gestión del encendido del LED y buzzer ante la detección local de una plaga (basado en el umbral).
- **Manejo de Comandos MQTT**:
  - Comando `"ON"`: Pone el control de los actuadores en modo automático (gestionado por el sensor).
  - Comando `"OFF"`: Desactiva los actuadores, independientemente de la lectura del sensor.
- **Comunicación con la API**: Envío regular de lecturas de sensores y recepción de órdenes de control.
- **Gestión de Conexiones**: Asegura la reconexión estable a la red WiFi y al broker MQTT.

---

## Perspectiva y Futuro

Este sistema representa una **solución modular, económica y autónoma** para el monitoreo de plagas, empleando una combinación eficiente de **HTTP (REST)** y **MQTT** para facilitar la integración con sistemas de visualización (dashboards) y la supervisión remota en tiempo real.

El proyecto es un cimiento para la **agricultura inteligente**, optimizando el uso de recursos y facilitando la toma de decisiones basada en datos.

**Ideas para el desarrollo futuro incluyen:**
- Integración de **cámaras para el reconocimiento visual de plagas**.
- Implementación de **Inteligencia Artificial para la predicción de eventos** de infestación.
- Conexión con **actuadores de tratamiento automático** (sistemas de aspersión, trampas inteligentes, etc.).