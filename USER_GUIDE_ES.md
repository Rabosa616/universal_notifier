# Universal Notifier - Guía de Usuario

Esta guía te acompaña a través de la instalación, configuración y puesta en marcha de Universal Notifier mediante la interfaz web de Home Assistant.

---

## 🚀 Instalación y Configuración Inicial

### Paso 1: Añadir la Integración
```
1. Settings → Devices & Services
2. Click en "+ ADD INTEGRATION"
3. Busca "Universal Notifier"
4. Click en la integración
```

### Paso 2: Completar el Asistente

#### 📝 Paso 2/1: Configuración General
**Campos disponibles:**

| Campo | Default | Descripción |
|-------|---------|-------------|
| Nombre del Asistente | *(vacío)* | Nombre del asistente (ej. "Home Assistant") |
| Formato Fecha/Hora | `%H:%M:%S` | Formato de marca temporal en las notificaciones |
| Incluir Hora | ✓ | Añade la hora actual al mensaje |
| Prefijo en Negrita | ✓ | Muestra el prefijo del mensaje en negrita |
| Ignorar Título de Voz | ✗ | Si está activado, los títulos de voz se omiten |
| Volumen de Prioridad | 0.9 (90%) | Volumen utilizado cuando `priority=True` |
| Entidad Person | *(vacío)* | Entidad `person.*` a vincular con esta integración |
| Días de Fin de Semana | Sáb, Dom | Días considerados fin de semana (Lun=0 ... Dom=6) |

#### 📝 Paso 2/2: No Molestar (DND)
**Campos disponibles:**

| Campo | Default Laborables | Default Fin de Semana |
|-------|-------------------|----------------------|
| Inicio DND | 23:00 | 00:00 |
| Fin DND | 06:00 | 08:00 |

> **Nota:** El modo No Molestar se configura de forma independiente para días laborables y fines de semana.

Click **SUBMIT**

#### 📝 Paso 2/3: Franjas Horarias

Configura las franjas horarias para **días laborables** y **fines de semana** por separado. Para cada franja, define una **hora de inicio** y un nivel de **volumen** (0.0 – 1.0).

**Días laborables:**

| Franja | Hora Default | Volumen Default |
|--------|-------------|-----------------|
| Mañana (`morning`) | 07:00 | 0.35 (35%) |
| Tarde (`afternoon`) | 12:00 | 0.40 (40%) |
| Noche (`evening`) | 19:00 | 0.30 (30%) |
| Noche tardía (`night`) | 21:30 | 0.10 (10%) |

**Fin de semana:**

| Franja | Hora Default | Volumen Default |
|--------|-------------|-----------------|
| Mañana (`morning`) | 08:00 | 0.30 (30%) |
| Tarde (`afternoon`) | 14:00 | 0.40 (40%) |
| Noche (`evening`) | 19:00 | 0.30 (30%) |
| Noche tardía (`night`) | 22:30 | 0.10 (10%) |

Click **SUBMIT**


#### 📝 Paso 2/4: Saludos

Define los saludos utilizados en las notificaciones de voz para cada franja horaria. Introduce **un saludo por línea** en el campo de texto.

**Valores por defecto:**

| Franja | Saludos |
|--------|---------|
| Mañana | Buenos días, Buen día, Hola, Qué tal |
| Tarde | Buenas tardes, Hola, Bienvenido de nuevo |
| Noche | Buenas noches, Buenas, Bienvenido a casa |
| Noche tardía | Buenas noches, Dulces sueños, Es tarde |

> **Nota:** Se elige un saludo aleatorio de la lista correspondiente cada vez que se envía una notificación.

Click **SUBMIT**

---

#### 📝 Paso 2/5: Menú de Canales
```
Canales configurados:
(ninguno todavía)

Action: [Añadir nuevo canal]
```
Click **SUBMIT**


#### 📝 Paso 2/6: Añadir un Canal (repetir para cada canal)

| Campo | Obligatorio | Descripción |
|-------|------------|-------------|
| Alias | ✓ | Nombre único del canal (ej. `alexa_salon`) |
| Servicio | ✓ | Nombre completo del servicio de Home Assistant (ej. `notify.alexa_media_echo_dot`) |
| Target | ✗ | Destino del servicio (dejar en blanco si no es necesario) |
| Es Voz | ✗ | Si el canal utiliza voz / TTS |
| Servicios Alternativos | ✗ | Objeto JSON con servicios de respaldo, ej. `{"fallback": "notify.xxx"}` |
| Media Player por Defecto | ✗ | Selector de entidad `media_player.*` para salida de audio |

**Ejemplo Alexa:**
```
Nombre del canal: alexa_salon
Servicio: notify.alexa_media_echo_dot
Target: (dejar en blanco)
Entity ID: (dejar en blanco)
Es Voz: ✓
```
Click **SUBMIT** → Vuelve al menú de canales

**Ejemplo Google Home:**
```
Nombre del canal: gh_cocina
Servicio: tts.google_translate_say
Target: tts.google_translate_es_es
Entity ID: media_player.cocina
Es Voz: ✓
```
Click **SUBMIT** → Vuelve al menú de canales

**Ejemplo Mobile App:**
```
Nombre del canal: mobile_app_user
Servicio: notify.mobile_app_phone
Target: (dejar en blanco)
Entity ID: (dejar en blanco)
Es Voz: ✗
```
Click **SUBMIT** → Vuelve al menú de canales

Una vez añadidos todos los canales:
```
Action: [Finalizar configuración]
```
Click **SUBMIT**

#### 📝 Paso 2/7: Finalizado
El asistente se cierra y la integración está lista para usar.

---

## 🎯 Verificación de la Instalación

### 1. Verificar la Integración
```
Settings → Devices & Services

Deberías ver:
┌─────────────────────────────────┐
│ Universal Notifier (Home Ass... │
│ 10 entities                     │
│ 1 service                       │
└─────────────────────────────────┘
```

### 2. Verificar las Entidades
```
Developer Tools → States

Busca:
✓ binary_sensor.universal_notifier_dnd
✓ sensor.universal_notifier_volume
✓ sensor.universal_notifier_family
✓ sensor.universal_notifier_default_player
✓ select.universal_notifier_priority_volume
✓ select.universal_notifier_text_format
✓ select.universal_notifier_notification_mode
✓ number.universal_notifier_voice_buffer
✓ text.universal_notifier_last_message_sent
✓ switch.universal_notifier_dnd_override

```

### 3. Verificar el Servicio
```
Developer Tools → Services

Busca:
✓ universal_notifier.send
```

### 4. Enviar una Notificación de Prueba
```yaml
# Developer Tools → Services
service: universal_notifier.send
data:
  message: "¡Notificación de prueba desde la interfaz!"
  targets:
    - alexa_salon
    - mobile_app_user
```

Click **CALL SERVICE**

Si recibes la notificación → **✅ ¡Instalación completada!**

---

## 🔧 Cambio de Configuración Tras la Instalación

### Acceder al Options Flow
```
Settings → Devices & Services
→ Universal Notifier
→ CONFIGURE
```

### Secciones Disponibles
```
○ Configuración general
  - Nombre del asistente
  - Formato de fecha
  - Incluir hora
  - Prefijo en negrita
  - Ignorar título
  - Volumen de prioridad
  - Entidad persona
  - Días de fin de semana

○ No Molestar
  - DND laborable inicio
  - DND laborable fin
  - DND fin de semana inicio
  - DND fin de semana fin

○ Franjas Horarias
  - Todos los horarios y niveles de volumen

○ Saludos
  - Saludos de mañana
  - Saludos de tarde
  - Saludos de noche
  - Saludos de noche tardía

○ Canales de Notificación
  - Añadir un nuevo canal
  - Eliminar canales existentes
  - Editar canales existentes
```

---

## 📊 Entidades Creadas

### 1. binary_sensor.universal_notifier_dnd
```yaml
State: on/off
Icon: mdi:bell-off (on) / mdi:bell-ring (off)
Attributes:
  dnd_start: "23:00"
  dnd_end: "07:00"
```

**Ejemplo de automatización:**
```yaml
trigger:
  - platform: state
    entity_id: binary_sensor.universal_notifier_dnd
    to: "off"
action:
  - service: universal_notifier.send
    data:
      message: "No Molestar finalizado, ¡buenos días!"
      targets: [alexa_salon]
```

### 2. sensor.universal_notifier_volume
```yaml
State: 35 (%)
Icon: mdi:volume-off/low/medium/high (dinámico)
Attributes:
  current_slot: "morning"
  raw_volume: 0.35
  time_slots: {...}
```

**Ejemplo de automatización:**
```yaml
trigger:
  - platform: state
    entity_id: sensor.universal_notifier_volume
action:
  - service: notify.persistent_notification
    data:
      message: "Volumen cambiado a {{ states('sensor.universal_notifier_volume') }}%"
```

### 3. select.universal_notifier_priority_volume
```yaml
State: "0.9"
Icon: mdi:volume-high
Attributes:
  decimal_value: 0.9
  percentage: "90%"
  description: "Volumen utilizado cuando priority=True"
Options:
  - "0.1" (10%)
  - "0.2" (20%)
  - ...
  - "1.0" (100%)
```

**Ejemplo de automatización:**
```yaml
# Establecer volumen al máximo por la noche
trigger:
  - platform: time
    at: "23:00:00"
action:
  - service: select.select_option
    target:
      entity_id: select.universal_notifier_priority_volume
    data:
      option: "1.0"
```

**Ejemplo de llamada de notificación:**
```yaml
service: universal_notifier.send
data:
  message: "¡EMERGENCIA!"
  targets: [alexa_salon]
  priority: true  # Utiliza el volumen de prioridad de la entidad select (1.0)
```

---

## 🎨 Ejemplo de Dashboard

```yaml
type: vertical-stack
cards:
  # Tarjeta de estado
  - type: entities
    title: Universal Notifier
    entities:
      - entity: binary_sensor.universal_notifier_dnd
        name: No Molestar
      - entity: sensor.universal_notifier_volume
        name: Volumen Actual
      - entity: select.universal_notifier_priority_volume
        name: Volumen de Prioridad
  
  # Indicador de volumen
  - type: gauge
    entity: sensor.universal_notifier_volume
    name: Volumen
    needle: true
    min: 0
    max: 100
    severity:
      green: 0
      yellow: 34
      orange: 67
      red: 90
  
  # Botón de prueba
  - type: button
    name: Notificación de Prueba
    icon: mdi:bell-ring
    tap_action:
      action: call-service
      service: universal_notifier.send
      service_data:
        message: "¡Prueba desde el dashboard!"
        targets:
          - alexa_salon
```

---

## 🐛 Resolución de Problemas

### La integración no aparece

**Solución:**
```bash
# 1. Verificar manifest.json
cat /config/custom_components/universal_notifier/manifest.json | grep config_flow
# Salida esperada: "config_flow": true

# 2. Comprobar los logs
tail -f /config/home-assistant.log | grep universal_notifier

# 3. Reiniciar Home Assistant
# Settings → System → Restart
```

### Faltan entidades

**Solución:**
```bash
# Confirmar que los archivos están en su lugar
ls -la /config/custom_components/universal_notifier/ | grep -E "(binary_sensor|sensor|select).py"

# Salida esperada:
# binary_sensor.py
# sensor.py
# select.py

# Comprobar los logs en busca de errores
grep -i "universal_notifier" /config/home-assistant.log | grep -i error
```

### Las llamadas al servicio fallan

**Solución:**
```yaml
# 1. Asegúrate de que al menos un canal está configurado
# Settings → Devices & Services → Universal Notifier → CONFIGURE

# 2. Prueba sencilla
service: universal_notifier.send
data:
  message: "Prueba"
  targets: [nombre_canal_configurado]

# 3. Comprobar los logs
tail -f /config/home-assistant.log | grep UniNotifier
```

### La entidad select no se actualiza

**Solución:**
```yaml
# Developer Tools → Services
service: homeassistant.reload_config_entry
target:
  entity_id: select.universal_notifier_priority_volume
```

---

## ✅ Lista de Verificación Final

### Instalación
- [ ] Integración añadida desde la interfaz
- [ ] Asistente completado
- [ ] Al menos un canal configurado
- [ ] 10 entidades visibles
- [ ] Servicio disponible

### Pruebas
- [ ] Notificación de prueba enviada correctamente
- [ ] Entidad DND funcionando
- [ ] Entidad Volumen funcionando
- [ ] Select de Volumen de Prioridad funcionando
- [ ] Options Flow accesible

---

## 🎉 ¡Listo!

Universal Notifier está instalado y listo para funcionar.

¡A disfrutar! 🚀
