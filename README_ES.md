# 📢 Universal Notifier
una nueva version de una appdaemon app de @caiosweet y @jumping2000

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge)](https://github.com/hacs/integration)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/jumping2000/universal_notifier?style=for-the-badge)
![GitHub Release Date](https://img.shields.io/github/release-date/jumping2000/universal_notifier?style=for-the-badge)
![GitHub stars](https://img.shields.io/github/stars/jumping2000/universal_notifier?style=for-the-badge)
![GitHub issues](https://img.shields.io/github/issues/jumping2000/universal_notifier?style=for-the-badge)
![License](https://img.shields.io/github/license/jumping2000/universal_notifier?style=for-the-badge)
![HA integration](https://img.shields.io/badge/Home%20Assistant-Integration-blue?style=for-the-badge)

> **🆕 Ultima version (v0.8.0):** DND separado laborables/fin de semana, soporte multi-target con valores separados por coma. Consulta el [Changelog](CHANGELOG.md) para mas detalles.
>
> [Guia de configuracion del usuario](USER_GUIDE_ES.md)
>
> 🇬🇧 [English Version](README.md)

### Invitame a un cafe y dame una estrella ✨!
[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/jumping)

[![Star History Chart](https://api.star-history.com/svg?repos=jumping2000/universal_notifier&type=Date)](https://star-history.com/#bytebase/star-history&Date)

___

**Universal Notifier** es un componente personalizado para Home Assistant que centraliza y mejora la gestion de notificaciones.

Transforma simples automatizaciones en un sistema de comunicacion "Smart Home" que conoce la hora del dia, respeta tu sueno (No Molestar - DND), saluda de forma natural y gestiona automaticamente el volumen de los asistentes de voz.

## 🚀 Caracteristicas Principales

* **Plataforma Unificada:** Un unico servicio (`universal_notifier.send`) para Telegram, App Movil, Alexa, Google Home, etc.
* **Notificaciones personalizadas** a varios destinatarios (por ejemplo, notificacion de alarma tanto a Telegram como a Alexa)
* **Voz vs Texto:** Diferencia automaticamente entre mensajes para leer (con prefijos como `[Jarvis - 12:30]`) y mensajes para hablar (solo texto limpio).
* **Franjas horarias & Volumen Inteligente:** Configura diferentes volumenes para Manana, Tarde, Noche y Madrugada. El componente ajusta el volumen *antes* de hablar.
* **No Molestar (DND):** Define horas de silencio para los asistentes de voz. Las notificaciones criticas (`priority: true`) se enviaran de todas formas.
* **Saludos Aleatorios:** "Buenos dias", "Buenas tardes", etc., elegidos aleatoriamente de listas personalizables.
* **Gestion de Comandos:** Soporte nativo para comandos de la Companion App (ej. `TTS`, `command_volume_level`) enviados en modo "RAW".
* **Cola Inteligente (FIFO):** Las notificaciones de voz se gestionan mediante un worker en segundo plano usando asyncio.Queue. Esto evita la superposicion de audio reproduciendo los mensajes de forma secuencial.
* **Snapshot y Restauracion:** El sistema guarda el estado (volumen, pista y app) de los reproductores multimedia antes de una notificacion e intenta restaurarlo despues de vaciar toda la cola.

### 📊 Monitorizacion & Diagnosticos - Entidades

| Entidad | Tipo | Descripcion |
|:---|:---|:---|
| **Volume** | Sensor | Muestra en tiempo real el porcentaje de volumen que se usara en la proxima notificacion, calculado automaticamente segun la franja horaria activa. Icono dinamico segun el nivel. Atributos extra: `current_slot`, `raw_volume`. |
| **Family** | Sensor | Rastrea el estado de presencia de la familia (`home` / `not_home`) basado en las entidades `person` configuradas. |
| **DND** | Binary Sensor | Indica si el modo No Molestar esta actualmente activo o inactivo. |
| **Voice Buffer** | Number | Tiempo de buffer ajustable (0.5-10.0 s, paso 0.5) para la reproduccion TTS, para garantizar la entrega completa del mensaje. Por defecto: 1.5 s. |
| **Priority Volume** | Select | Establece el nivel de volumen para las notificaciones prioritarias. Opciones: de 0.1 a 1.0. |
| **Text Format** | Select | Selecciona el formato de texto para las notificaciones: `html` o `markdown`. |
| **Notification Mode** | Select | Controla el enrutamiento de notificaciones segun la presencia: `Normal` (todas pasan), `Voice home` (voz solo cuando estas en casa), `Text home` (solo texto, sin voz). |
| **Default Media Players** | Sensor | Muestra los reproductores multimedia predeterminados configurados para los canales de voz. Estado: numero de canales con valor por defecto. Atributos: mapa `{alias_canal: media_player.xxx}`. |
| **DND Override** | Switch | Fuerza el modo No Molestar independientemente del horario programado. Util para activar manualmente el modo silencioso en cualquier momento. |
| **Last Message Sent** | Text | Almacena el texto de la ultima notificacion enviada (max 255 caracteres). Se actualiza automaticamente despues de cada llamada a `universal_notifier.send`. |

___

## 🛠️ Instalacion

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=jumping2000&repository=universal_notifier&category=Integration)

<details>
<summary>Haz clic para mostrar las instrucciones de instalacion</summary>
<ol>
<li>Instalar los archivos:</li>
<ul>
<li><u>Mediante HACS:</u><br>
En el panel de HACS, busca 'Universal Notifier', abre el repositorio y haz clic en 'Download'.</li>
<li><u>Manualmente:</u><br>
Descarga la <a href="https://github.com/jumping2000/universal_notifier/releases">ultima version</a> como archivo zip y extraela en la carpeta `custom_components` de tu instalacion de HA.</li>
</ul>
<li>Reinicia HA para cargar la integracion.</li>
<li>Ve a Ajustes -> Dispositivos y servicios y haz clic en 'AGREGAR INTEGRACION'. Busca Universal Notifier y haz clic para agregarlo.</li>
<li>La integracion Universal Notifier esta lista para la configuracion YAML.</li>
</ol>
</details>

## 🔗 Requisitos Previos
<details>
  <summary>Haz clic para expandir</summary>

Antes de configurar Universal Notifier, asegurate de haber instalado y configurado las integraciones de notificacion subyacentes que planeas utilizar:
* **Google Home / TTS**: Instala la integracion Google Translate [Text-to-Speech (TTS)](https://www.home-assistant.io/integrations/tts) para habilitar los anuncios de voz en dispositivos Google Assistant.
* **Alexa / Echo Devices**: Instala el [Alexa Media Player Custom Component](https://github.com/alandtse/alexa_media_player) (mediante HACS) para permitir que Home Assistant envie anuncios y configure el volumen en dispositivos Echo.
* **Telegram**: Configura e instala la integracion [Telegram Bot](https://www.home-assistant.io/integrations/telegram_bot/) para enviar mensajes visuales.
* **Mobile App**: Asegurate de que la integracion [Mobile App](https://companion.home-assistant.io/) este activa y configurada para tus dispositivos (normalmente se configura automaticamente al iniciar sesion desde la app).

Este componente actua como un "router"; los servicios de destino deben estar disponibles para funcionar correctamente.
</details>

## ⚙️ Configuracion (UI)

Universal Notifier es completamente configurable desde la interfaz de usuario de Home Assistant. No se necesita ninguna configuracion YAML.

<details>
  <summary>Haz clic para expandir</summary>

### Configuracion Inicial

Despues de instalar la integracion, ve a **Ajustes > Dispositivos y servicios > Agregar integracion** y busca **Universal Notifier**. El asistente de configuracion te guiara a traves de los siguientes pasos:

#### Paso 1 — Ajustes Globales & No Molestar
| Ajuste | Descripcion | Por defecto |
|:---|:---|:---|
| Nombre del Asistente | Nombre mostrado en los prefijos de los mensajes de texto (ej. `[Jarvis - 12:30]`) | `Jarvis` |
| Formato de Hora | Formato strftime para el prefijo de hora | `%H:%M` |
| Incluir Hora en el Prefijo | Si mostrar la hora en las notificaciones de texto | `true` |
| Prefijo en Negrita | Si poner en negrita el nombre del asistente y la hora | `true` |
| Volumen Prioritario | Volumen usado con `priority: true` (0.0 - 1.0) | `0.9` |
| Entidades Person | Entidades person opcionales para la deteccion de presencia | — |
| Inicio DND | Hora de inicio del modo No Molestar (HH:MM) | `23:00` |
| Fin DND | Hora de fin del modo No Molestar (HH:MM) | `06:00` |

#### Paso 2 — Franjas Horarias
Configura la hora de inicio y el volumen TTS predeterminado para cada periodo del dia.

| Franja Horaria | Inicio Predeterminado | Volumen Predeterminado |
|:---|:---|:---|
| Manana | 07:00 | 0.35 |
| Tarde | 12:00 | 0.4 |
| Noche | 19:00 | 0.3 |
| Madrugada | 22:00 | 0.1 |

#### Paso 3 — Saludos
Introduce un saludo por linea para cada franja horaria. Se elegira un saludo aleatorio cada vez que se envie una notificacion.

#### Paso 4 — Primer Canal (obligatorio)
Debes agregar al menos un canal de notificacion para completar la configuracion. Cada canal requiere:

| Campo | Descripcion |
|:---|:---|
| Alias | Un nombre unico para el canal (ej. `alexa_salon`) |
| Service | El servicio HA a llamar en formato `dominio.servicio` (ej. `notify.mobile_app_pixel`) |
| Target | entity_id de destino (opcional, separado por coma para multiples destinos) |
| Canal de Voz | Activar para dispositivos TTS (aplica volumen, DND y logica de saludos) |
| Servicios Alternativos | Diccionario JSON opcional para servicios alternativos (ej. foto/video de Telegram) |

### Modificar la Configuracion

Despues de la configuracion inicial, ve a **Ajustes > Dispositivos y servicios > Universal Notifier > Configurar** para acceder al menu de opciones:

- **Ajustes Globales** — Editar nombre del asistente, formato de hora, opciones de prefijo y volumen prioritario
- **No Molestar** — Cambiar horas de inicio/fin del DND
- **Franjas Horarias** — Ajustar horas de inicio y volumenes para cada periodo
- **Saludos** — Personalizar los saludos para cada franja horaria
- **Canales** — Agregar o eliminar canales de notificacion

### Pequenos consejos
- si olvidas los canales configurados, ve a `Integraciones` - `Universal Notifier` - `Configurar` - `Canales` - `Eliminar canal`
- para foto y video de Telegram agrega en la configuracion del canal:
```
{
  "photo": {"service": "telegram_bot.send_photo"},
  "video": {"service": "telegram_bot.send_video"}
}
```


</details>

## 🎯 Referencia de Campos del Servicio
<details>
  <summary>Haz clic para expandir</summary>

|Campo|Tipo|Obligatorio|Descripcion|
|:---|:---|:---|:---|
|message|string|Si|El texto principal de la notificacion.|
|targets|list|Si|Lista de alias de canales definidos en configuration.yaml.|
|title|string|No|Titulo de la notificacion (soportado por Notify y Mobile App).|
|data|dict|No|Datos extra genericos aplicados a TODOS los servicios subyacentes.|
|target_data|dict|No|Diccionario {alias_target: {datos_especificos}} para sobreescrituras dirigidas.|
|priority|bool|No|Si es true, ignora el DND y establece volumen alto (por defecto 0.9).|
|skip_greeting|bool|No|Si es true, no agrega el saludo basado en la hora (ej. Buenos dias).|
|skip_assistant_name|bool|No|Si es true, omite el nombre del asistente del prefijo visual.|
|include_time|bool|No|Sobreescribe la configuracion para incluir/excluir la hora en el prefijo visual.|
|ignore_title_voice|bool|No|Si es true, ignora el titulo para las notificaciones de voz (TTS/canales de voz).|
|bold_prefix|bool|No|Sobreescribe la configuracion para poner en negrita el nombre del asistente y la hora.|
|assistant_name|string|No|Sobreescribe el nombre global del asistente.|
|override_greetings|dict|No|Sobreescribe los saludos predeterminados.|

</details>

## 📝 Ejemplos de Uso
<details>
  <summary>Haz clic para expandir</summary>

#### 1. Notificacion Estandar (Volumen Automatico)
Si se envia a las 15:00, usara el volumen de la tarde (0.60). Si se envia a las 02:00 (DND activo), Alexa sera omitida, pero Telegram recibira el mensaje.

```yaml
action: universal_notifier.send
data:
  message: "La colada ha terminado."
  targets:
    - alexa_salon
    - telegram_admin
```

#### 2. Notificacion Prioritaria (Ignora DND y establece Volumen al 90%)
Usa el flag priority para alertas criticas.

```yaml
action: universal_notifier.send
data:
  title: "ALERTA CRITICA"
  message: "Fuga de agua detectada, valvula cerrada!"
  priority: true        # <--- FUERZA EL ENVIO Y VOLUMEN MAXIMO (0.9)
  skip_greeting: true   # <--- Evita saludos como "Buenas noches" durante una alarma
  targets:
    - alexa_salon
    - telegram_bob
```

#### 3. Comandos de Companion App (Mensajes RAW)
Si el mensaje es un comando reconocido (como "TTS") o empieza con *command_*, los saludos y prefijos se eliminan automaticamente.

```yaml
action: universal_notifier.send
data:
  message: "TTS" # El componente envia "TTS" RAW, sin prefijos.
  targets:
    - my_android
  target_data:
    my_android:
      tts_text: "El cartero esta en la puerta."
      media_stream: alarm_stream_max
      clickAction: /lovelace/main
```

#### 4. Multiples destinos
Como enviar a multiples destinos.

```yaml
action: universal_notifier.send
data:
  message: La lavadora ha terminado su ciclo.
  title: Aviso Lavadora
  priority: true
  targets:
    - google_home
    - telegram_bob
    - mobile_bob
  target_data:
    google_home:
      entity_id: media_player.cocina
      volume: 0.3
    mobile_bob:
      image: "https://www.home-assistant.io/images/default-social.png"
      color: red
      channel: "lavadora-alert"
    telegram_bob:
      type: photo
      url: "https://www.home-assistant.io/images/default-social.png"
```

</details>

## 🪲 Solucion de Problemas
<details>
  <summary>Haz clic para expandir</summary>
  
Para depuracion, agrega en *configuration.yaml*:

```yaml
logger:
  default: info
  logs:
    custom_components.universal_notifier: debug
```

</details>
