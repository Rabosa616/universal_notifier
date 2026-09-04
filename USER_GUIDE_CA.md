# Universal Notifier - Guia d'Usuari

Aquesta guia et guia a través de la instal·lació, configuració i posada en marxa de Universal Notifier mitjançant la interfície web de Home Assistant.

---

## 🚀 Instal·lació i Configuració Inicial

### Pas 1: Afegeix la Integració
```
1. Settings → Devices & Services
2. Clica "+ ADD INTEGRATION"
3. Cerca "Universal Notifier"
4. Clica la integració
```

### Pas 2: Completa l'Assistent

#### 📝 Pas 2/1: Configuració General
**Camps disponibles:**

| Camp | Per defecte | Descripció |
|------|-------------|------------|
| Nom de l'Assistent | *(buit)* | Nom de l'assistent (p. ex. "Home Assistant") |
| Format Data/Hora | `%H:%M:%S` | Format de marca temporal a les notificacions |
| Inclou Hora | ✓ | Afegeix l'hora actual als missatges |
| Prefix en Negreta | ✓ | Formata el prefix del missatge en negreta |
| Ignora Títol de Veu | ✗ | Si està actiu, els títols de veu s'ignoren |
| Volum de Prioritat | 0.9 (90%) | Volum utilitzat quan `priority=True` |
| Entitat Person | *(buit)* | Entitat `person.*` per associar a aquesta integració |
| Dies de Cap de Setmana | Dis, Diu | Dies considerats cap de setmana (Dll=0 ... Diu=6) |

#### 📝 Pas 2/2: No Molestar (DND)
**Camps disponibles:**

| Camp | Per defecte Laborable | Per defecte Cap de Setmana |
|------|----------------------|---------------------------|
| Inici DND | 23:00 | 00:00 |
| Fi DND | 06:00 | 08:00 |

> **Nota:** El DND es configura de manera independent per a dies laborables i caps de setmana.

Clica **SUBMIT**

#### 📝 Pas 2/3: Franges Horàries

Configura les franges horàries per a **dies laborables** i **caps de setmana** de manera independent. Per a cada franja, defineix una **hora d'inici** i un nivell de **volum** (0.0 – 1.0).

**Dies Laborables:**

| Franja | Hora per Defecte | Volum per Defecte |
|--------|-----------------|-------------------|
| Matí (`morning`) | 07:00 | 0.35 (35%) |
| Tarda (`afternoon`) | 12:00 | 0.40 (40%) |
| Vespre (`evening`) | 19:00 | 0.30 (30%) |
| Nit (`night`) | 21:30 | 0.10 (10%) |

**Cap de Setmana:**

| Franja | Hora per Defecte | Volum per Defecte |
|--------|-----------------|-------------------|
| Matí (`morning`) | 08:00 | 0.30 (30%) |
| Tarda (`afternoon`) | 14:00 | 0.40 (40%) |
| Vespre (`evening`) | 19:00 | 0.30 (30%) |
| Nit (`night`) | 22:30 | 0.10 (10%) |

Clica **SUBMIT**


#### 📝 Pas 2/4: Salutacions

Defineix les salutacions utilitzades a les notificacions de veu per a cada franja horària. Introdueix **una salutació per línia** al camp de text.

**Per defecte:**

| Franja | Salutacions |
|--------|-------------|
| Matí | Bon dia, Bon matí, Hola, Bona jornada |
| Tarda | Bona tarda, Hola, Ben tornat |
| Vespre | Bona nit, Bona vetllada, Benvingut a casa |
| Nit | Bona nit, Dolços somnis, És tard |

> **Nota:** Es tria una salutació aleatòria de la llista corresponent cada vegada que s'envia una notificació.

Clica **SUBMIT**

---

#### 📝 Pas 2/5: Menú de Canals
```
Canals configurats:
(cap encara)

Action: [Afegeix un nou canal]
```
Clica **SUBMIT**


#### 📝 Pas 2/6: Afegeix un Canal (repeteix per a cada canal)

| Camp | Obligatori | Descripció |
|------|-----------|------------|
| Àlies | ✓ | Nom únic del canal (p. ex. `alexa_sala_estar`) |
| Servei | ✓ | Nom complet del servei de Home Assistant (p. ex. `notify.alexa_media_echo_dot`) |
| Target | ✗ | Objectiu del servei (deixa en blanc si no cal) |
| És Veu | ✗ | Si el canal utilitza veu / TTS |
| Serveis Alternatius | ✗ | Objecte JSON amb serveis alternatius, p. ex. `{"fallback": "notify.xxx"}` |
| Reproductor Multimèdia per Defecte | ✗ | Selector d'entitat `media_player.*` per a la sortida d'àudio |

**Exemple Alexa:**
```
Nom del canal: alexa_sala_estar
Servei: notify.alexa_media_echo_dot
Target: (deixa en blanc)
Entity ID: (deixa en blanc)
És Veu: ✓
```
Clica **SUBMIT** → Torna al menú de canals

**Exemple Google Home:**
```
Nom del canal: gh_cuina
Servei: tts.google_translate_say
Target: tts.google_translate_ca_es
Entity ID: media_player.cuina
És Veu: ✓
```
Clica **SUBMIT** → Torna al menú de canals

**Exemple Mobile App:**
```
Nom del canal: mobile_app_user
Servei: notify.mobile_app_phone
Target: (deixa en blanc)
Entity ID: (deixa en blanc)
És Veu: ✗
```
Clica **SUBMIT** → Torna al menú de canals

Un cop afegits tots els canals:
```
Action: [Finalitza la configuració]
```
Clica **SUBMIT**

#### 📝 Pas 2/7: Fet
L'assistent es tanca i la integració està llesta per utilitzar!

---

## 🎯 Verificació de la Instal·lació

### 1. Comprova la Integració
```
Settings → Devices & Services

Hauries de veure:
┌─────────────────────────────────┐
│ Universal Notifier (Home Ass... │
│ 10 entities                     │
│ 1 service                       │
└─────────────────────────────────┘
```

### 2. Comprova les Entitats
```
Developer Tools → States

Cerca:
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

### 3. Comprova el Servei
```
Developer Tools → Services

Cerca:
✓ universal_notifier.send
```

### 4. Envia una Notificació de Prova
```yaml
# Developer Tools → Services
service: universal_notifier.send
data:
  message: "Notificació de prova des de la UI!"
  targets:
    - alexa_sala_estar
    - mobile_app_user
```

Clica **CALL SERVICE**

Si reps la notificació → **✅ Instal·lació completada!**

---

## 🔧 Canvi de Configuració Després de la Configuració Inicial

### Accedir a l'Options Flow
```
Settings → Devices & Services
→ Universal Notifier
→ CONFIGURE
```

### Seccions Disponibles
```
○ Configuració general
  - Nom de l'assistent
  - Format de data
  - Inclou hora
  - Prefix en negreta
  - Ignora títol
  - Volum de prioritat
  - Entitat persona
  - Dies de cap de setmana

○ No Molestar
  - DND laborable inici
  - DND laborable fi
  - DND cap de setmana inici
  - DND cap de setmana fi

○ Franges Horàries
  - Tots els horaris i nivells de volum

○ Salutacions
  - Salutacions del matí
  - Salutacions de la tarda
  - Salutacions del vespre
  - Salutacions de la nit

○ Canals de Notificació
  - Afegeix un nou canal
  - Elimina canals existents
  - Edita canals existents
```

---

## 📊 Entitats Creades

### 1. binary_sensor.universal_notifier_dnd
```yaml
State: on/off
Icon: mdi:bell-off (on) / mdi:bell-ring (off)
Attributes:
  dnd_start: "23:00"
  dnd_end: "07:00"
```

**Exemple d'automatització:**
```yaml
trigger:
  - platform: state
    entity_id: binary_sensor.universal_notifier_dnd
    to: "off"
action:
  - service: universal_notifier.send
    data:
      message: "DND finalitzat — bon dia!"
      targets: [alexa_sala_estar]
```

### 2. sensor.universal_notifier_volume
```yaml
State: 35 (%)
Icon: mdi:volume-off/low/medium/high (dinàmic)
Attributes:
  current_slot: "morning"
  raw_volume: 0.35
  time_slots: {...}
```

**Exemple d'automatització:**
```yaml
trigger:
  - platform: state
    entity_id: sensor.universal_notifier_volume
action:
  - service: notify.persistent_notification
    data:
      message: "El volum ha canviat a {{ states('sensor.universal_notifier_volume') }}%"
```

### 3. select.universal_notifier_priority_volume
```yaml
State: "0.9"
Icon: mdi:volume-high
Attributes:
  decimal_value: 0.9
  percentage: "90%"
  description: "Volum utilitzat quan priority=True"
Options:
  - "0.1" (10%)
  - "0.2" (20%)
  - ...
  - "1.0" (100%)
```

**Exemple d'automatització:**
```yaml
# Estableix el volum al màxim a la nit
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

**Exemple de crida de notificació:**
```yaml
service: universal_notifier.send
data:
  message: "EMERGÈNCIA!"
  targets: [alexa_sala_estar]
  priority: true  # Utilitza el volum de prioritat de l'entitat select (1.0)
```

---

## 🎨 Exemple de Dashboard

```yaml
type: vertical-stack
cards:
  # Targeta d'estat
  - type: entities
    title: Universal Notifier
    entities:
      - entity: binary_sensor.universal_notifier_dnd
        name: No Molestar
      - entity: sensor.universal_notifier_volume
        name: Volum Actual
      - entity: select.universal_notifier_priority_volume
        name: Volum de Prioritat
  
  # Indicador de volum
  - type: gauge
    entity: sensor.universal_notifier_volume
    name: Volum
    needle: true
    min: 0
    max: 100
    severity:
      green: 0
      yellow: 34
      orange: 67
      red: 90
  
  # Botó de prova
  - type: button
    name: Notificació de Prova
    icon: mdi:bell-ring
    tap_action:
      action: call-service
      service: universal_notifier.send
      service_data:
        message: Prova des del dashboard!
        targets:
          - alexa_sala_estar
```

---

## 🐛 Resolució de Problemes

### La integració no apareix

**Solució:**
```bash
# 1. Verifica manifest.json
cat /config/custom_components/universal_notifier/manifest.json | grep config_flow
# Sortida esperada: "config_flow": true

# 2. Comprova els registres
tail -f /config/home-assistant.log | grep universal_notifier

# 3. Reinicia Home Assistant
# Settings → System → Restart
```

### Les entitats no apareixen

**Solució:**
```bash
# Confirma que els fitxers són al seu lloc
ls -la /config/custom_components/universal_notifier/ | grep -E "(binary_sensor|sensor|select).py"

# Sortida esperada:
# binary_sensor.py
# sensor.py
# select.py

# Comprova els registres per trobar errors
grep -i "universal_notifier" /config/home-assistant.log | grep -i error
```

### Les crides al servei fallen

**Solució:**
```yaml
# 1. Assegura't que almenys un canal està configurat
# Settings → Devices & Services → Universal Notifier → CONFIGURE

# 2. Prova un test senzill
service: universal_notifier.send
data:
  message: "Test"
  targets: [nom_del_canal_configurat]

# 3. Comprova els registres
tail -f /config/home-assistant.log | grep UniNotifier
```

### L'entitat select no s'actualitza

**Solució:**
```yaml
# Developer Tools → Services
service: homeassistant.reload_config_entry
target:
  entity_id: select.universal_notifier_priority_volume
```

---

## ✅ Llista de Verificació Final

### Instal·lació
- [ ] Integració afegida des de la UI
- [ ] Assistent completat
- [ ] Almenys un canal configurat
- [ ] 10 entitats visibles
- [ ] Servei disponible

### Proves
- [ ] Notificació de prova enviada correctament
- [ ] Entitat DND funcional
- [ ] Entitat Volum funcional
- [ ] Select de Volum de Prioritat funcional
- [ ] Options Flow accessible

---

## 🎉 Fet!

Universal Notifier està instal·lat i llest per funcionar!

Gaudeix-ne! 🚀
