# 📢 Universal Notifier
Una nova versio d'una appdaemon app de @caiosweet i @jumping2000

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge)](https://github.com/hacs/integration)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/jumping2000/universal_notifier?style=for-the-badge)
![GitHub Release Date](https://img.shields.io/github/release-date/jumping2000/universal_notifier?style=for-the-badge)
![GitHub stars](https://img.shields.io/github/stars/jumping2000/universal_notifier?style=for-the-badge)
![GitHub issues](https://img.shields.io/github/issues/jumping2000/universal_notifier?style=for-the-badge)
![License](https://img.shields.io/github/license/jumping2000/universal_notifier?style=for-the-badge)
![HA integration](https://img.shields.io/badge/Home%20Assistant-Integration-blue?style=for-the-badge)

> **🆕 Ultima versio (v0.8.0):** DND separat dies feiners/cap de setmana, suport multi-target amb valors separats per coma. Consulta el [Changelog](CHANGELOG.md) per als detalls.
>
> [Guia d'usuari per a la configuracio](USER_GUIDE_CA.md)
>
> 🇬🇧 [English Version](README.md)

### Compra'm un cafe i dona'm una estrella ✨!
[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/jumping)

___

**Universal Notifier** es un component personalitzat per a Home Assistant que centralitza i millora la gestio de les notificacions.

Transforma automatitzacions simples en un sistema de comunicacio "Smart Home" que coneix l'hora del dia, respecta el teu descans (No Molestar - DND), saluda de manera natural i gestiona automaticament el volum dels assistents de veu.

## 🚀 Caracteristiques Principals

* **Plataforma Unificada:** Un unic servei (`universal_notifier.send`) per a Telegram, App Mobil, Alexa, Google Home, etc.
* **Notificacions personalitzades** a diversos destinataris (p. ex., notificacio d'alarma tant a Telegram com a Alexa)
* **Veu vs Text:** Diferencia automaticament entre missatges per llegir (amb prefixos com `[Jarvis - 12:30]`) i missatges per pronunciar (nomes text net).
* **Franges horaries & Volum Intel·ligent:** Estableix volums diferents per al Mati, Tarda, Vespre i Nit. El component ajusta el volum *abans* de parlar.
* **No Molestar (DND):** Defineix hores de silenci per als assistents de veu. Les notificacions critiques (`priority: true`) continuaran passant.
* **Salutacions Aleatories:** "Bon dia", "Bona tarda", etc., triades aleatoriament de llistes personalitzables.
* **Gestio de Comandes:** Suport natiu per a comandes de Companion App (p. ex., `TTS`, `command_volume_level`) enviades en mode "RAW".
* **Cua Intel·ligent (FIFO):** Les notificacions de veu son gestionades per un worker en segon pla mitjancant asyncio.Queue. Aixo evita la superposicio d'audio reproduint els missatges sequencialment.
* **Snapshot i Restauracio:** El sistema desa l'estat (volum, pista i app) dels reproductors multimedia abans d'una notificacio i intenta restaurar-lo despres que tota la cua s'hagi buidat.

### 📊 Monitoratge i Diagnostics - Entitats

| Entitat | Tipus | Descripcio |
|:---|:---|:---|
| **Volume** | Sensor | Mostra en temps real el percentatge de volum que s'utilitzara per a la propera notificacio, calculat automaticament segons la franja horaria activa. Icona dinamica segons el nivell. Atributs extra: `current_slot`, `raw_volume`. |
| **Family** | Sensor | Segueix l'estat de presencia de la familia (`home` / `not_home`) segons les entitats `person` configurades. |
| **DND** | Binary Sensor | Indica si el mode No Molestar esta actualment actiu o inactiu. |
| **Voice Buffer** | Number | Temps de buffer ajustable (0.5-10.0 s, pas 0.5) per a la reproduccio TTS, per garantir el lliurament complet del missatge. Per defecte: 1.5 s. |
| **Priority Volume** | Select | Estableix el nivell de volum per a les notificacions prioritaries. Opcions: de 0.1 a 1.0. |
| **Text Format** | Select | Selecciona el format de text per a les notificacions: `html` o `markdown`. |
| **Notification Mode** | Select | Controla l'encaminament de les notificacions segons la presencia: `Normal` (totes passen), `Voice home` (veu nomes si ets a casa), `Text home` (nomes text, sense veu). |
| **Default Media Players** | Sensor | Mostra els reproductors multimedia predeterminats configurats per als canals de veu. Estat: nombre de canals amb un valor predeterminat. Atributs: mapa `{alias_canal: media_player.xxx}`. |
| **DND Override** | Switch | Forca el mode No Molestar independentment de l'horari programat. Util per activar manualment el mode silencio en qualsevol moment. |
| **Last Message Sent** | Text | Emmagatzema el text de l'ultima notificacio enviada (max 255 caracters). S'actualitza automaticament despres de cada crida a `universal_notifier.send`. |

___

## 🛠️ Instal·lacio

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=jumping2000&repository=universal_notifier&category=Integration)

<details>
<summary>Fes clic per mostrar les instruccions d'instal·lacio</summary>
<ol>
<li>Instal·lar els fitxers:</li>
<ul>
<li><u>Mitjancant HACS:</u><br>
Al panell HACS, cerca 'Universal Notifier', obre el repositori i fes clic a 'Download'.</li>
<li><u>Manualment:</u><br>
Descarrega l'<a href="https://github.com/jumping2000/universal_notifier/releases">ultima versio</a> com a fitxer zip i extreu-la a la carpeta `custom_components` de la teva instal·lacio de HA.</li>
</ul>
<li>Reinicia HA per carregar la integracio.</li>
<li>Ves a Configuracio -> Dispositius i serveis i fes clic a 'AFEGEIX INTEGRACIO'. Cerca Universal Notifier i fes clic per afegir-lo.</li>
<li>La integracio Universal Notifier esta llesta per a la configuracio YAML.</li>
</ol>
</details>

## 🔗 Prerequisits
<details>
  <summary>Fes clic per expandir</summary>

Abans de configurar Universal Notifier, assegura't d'haver instal·lat i configurat les integracions de notificacio subjacents que pensis utilitzar:
* **Google Home / TTS**: Instal·la la integracio Google Translate [Text-to-Speech (TTS)](https://www.home-assistant.io/integrations/tts) per habilitar els anuncis de veu als dispositius Google Assistant.
* **Alexa / Echo Devices**: Instal·la l'[Alexa Media Player Custom Component](https://github.com/alandtse/alexa_media_player) (mitjancant HACS) per permetre a Home Assistant enviar anuncis i establir el volum als dispositius Echo.
* **Telegram**: Configura i instal·la la integracio [Telegram Bot](https://www.home-assistant.io/integrations/telegram_bot/) per enviar missatges visuals.
* **Mobile App**: Assegura't que la integracio [Mobile App](https://companion.home-assistant.io/) estigui activa i configurada per als teus dispositius (normalment es configura automaticament quan inicies sessio mitjancant l'app).

Aquest component actua com un "router"; els serveis de destinacio han d'estar disponibles perque funcioni correctament.
</details>

## ⚙️ Configuracio (UI)

Universal Notifier es completament configurable des de la interficie d'usuari de Home Assistant. No cal cap configuracio YAML.

<details>
  <summary>Fes clic per expandir</summary>

### Configuracio Inicial

Despres d'instal·lar la integracio, ves a **Configuracio > Dispositius i serveis > Afegeix integracio** i cerca **Universal Notifier**. L'assistent de configuracio et guiara pels passos seguents:

#### Pas 1 — Configuracio Global i No Molestar
| Opcio | Descripcio | Per defecte |
|:---|:---|:---|
| Nom de l'Assistent | Nom mostrat als prefixos dels missatges de text (p. ex. `[Jarvis - 12:30]`) | `Jarvis` |
| Format d'Hora | Format strftime per al prefix temporal | `%H:%M` |
| Incloure Hora al Prefix | Si mostrar l'hora a les notificacions de text | `true` |
| Prefix en Negreta | Si posar en negreta el nom de l'assistent i l'hora | `true` |
| Volum Prioritari | Volum utilitzat amb `priority: true` (0.0 - 1.0) | `0.9` |
| Entitats Person | Entitats person opcionals per a la deteccio de presencia | — |
| Inici DND | Hora d'inici del mode No Molestar (HH:MM) | `23:00` |
| Fi DND | Hora de fi del mode No Molestar (HH:MM) | `06:00` |

#### Pas 2 — Franges Horaries
Estableix l'hora d'inici i el volum TTS predeterminat per a cada periode del dia.

| Franja Horaria | Inici Predeterminat | Volum Predeterminat |
|:---|:---|:---|
| Mati | 07:00 | 0.35 |
| Tarda | 12:00 | 0.4 |
| Vespre | 19:00 | 0.3 |
| Nit | 22:00 | 0.1 |

#### Pas 3 — Salutacions
Introdueix una salutacio per linia per a cada franja horaria. Una salutacio aleatoria sera triada cada vegada que s'envii una notificacio.

#### Pas 4 — Primer Canal (obligatori)
Has d'afegir almenys un canal de notificacio per completar la configuracio. Cada canal requereix:

| Camp | Descripcio |
|:---|:---|
| Alias | Un nom unic per al canal (p. ex. `alexa_sala_estar`) |
| Service | El servei HA a cridar en format `domini.servei` (p. ex. `notify.mobile_app_pixel`) |
| Target | entity_id de destinacio (opcional, separat per coma per a multiples destinacions) |
| Canal de Veu | Habilita per a dispositius TTS (aplica volum, DND i logica de salutacions) |
| Serveis Alternatius | Diccionari JSON opcional per a serveis alternatius (p. ex. foto/video de Telegram) |

### Edicio de la Configuracio

Despres de la configuracio inicial, ves a **Configuracio > Dispositius i serveis > Universal Notifier > Configura** per accedir al menu d'opcions:

- **Configuracio Global** — Edita nom de l'assistent, format d'hora, opcions de prefix i volum prioritari
- **No Molestar** — Modifica les hores d'inici/fi del DND
- **Franges Horaries** — Ajusta les hores d'inici i els volums per a cada periode
- **Salutacions** — Personalitza les salutacions per a cada franja horaria
- **Canals** — Afegeix o elimina canals de notificacio

### Petits consells
- si oblides els canals configurats, ves a `Integracions` - `Universal Notifier` - `Configura` - `Canals` - `Elimina canal`
- per a fotos i videos de Telegram afegeix a la configuracio del canal:
```
{
  "photo": {"service": "telegram_bot.send_photo"},
  "video": {"service": "telegram_bot.send_video"}
}
```

</details>

## 🎯 Referencia de Camps del Servei
<details>
  <summary>Fes clic per expandir</summary>

|Camp|Tipus|Obligatori|Descripcio|
|:---|:---|:---|:---|
|message|string|Si|El text principal de la notificacio.|
|targets|list|Si|Llista dels alias dels canals definits a configuration.yaml.|
|title|string|No|Titol de la notificacio (suportat per Notify i Mobile App).|
|data|dict|No|Dades extra generiques aplicades a TOTS els serveis subjacents.|
|target_data|dict|No|Diccionari {alias_target: {dades_especifiques}} per a sobreescriptures dirigides.|
|priority|bool|No|Si es true, ignora el DND i estableix volum alt (per defecte 0.9).|
|skip_greeting|bool|No|Si es true, no afegeix la salutacio basada en l'hora (p. ex. Bon dia).|
|skip_assistant_name|bool|No|Si es true, omet el nom de l'assistent del prefix visual.|
|include_time|bool|No|Sobreescriu la configuracio per incloure/excloure l'hora al prefix visual.|
|ignore_title_voice|bool|No|Si es true, ignora el titol per a les notificacions de veu (TTS/canals de veu).|
|bold_prefix|bool|No|Sobreescriu la configuracio per posar en negreta el nom de l'assistent i l'hora.|
|assistant_name|string|No|Sobreescriu el nom global de l'assistent.|
|override_greetings|dict|No|Sobreescriu les salutacions predeterminades.|

</details>

## 📝 Exemples d'Us
<details>
  <summary>Fes clic per expandir</summary>

#### 1. Notificacio Estandard (Volum Automatic)
Si s'envia a les 15:00, utilitzara el volum de la tarda (0.60). Si s'envia a les 02:00 (DND actiu), Alexa sera omesa, pero Telegram rebra el missatge.

```yaml
action: universal_notifier.send
data:
  message: "La bugada ha acabat."
  targets:
    - alexa_sala_estar
    - telegram_admin
```

#### 2. Notificacio Prioritaria (Ignora DND i estableix Volum al 90%)
Utilitza el flag priority per a alertes critiques.

```yaml
action: universal_notifier.send
data:
  title: "ALERTA CRITICA"
  message: "Fuita d'aigua detectada, valvula tancada!"
  priority: true        # <--- FORCA L'ENVIAMENT I VOLUM MAXIM (0.9)
  skip_greeting: true   # <--- Evita salutacions com "Bona nit" durant una alarma
  targets:
    - alexa_sala_estar
    - telegram_bob
```

#### 3. Comandes de Companion App (Missatges RAW)
Si el missatge es una comanda reconeguda (com "TTS") o comenca amb *command_*, les salutacions i prefixos s'eliminen automaticament.

```yaml
action: universal_notifier.send
data:
  message: "TTS" # El component envia "TTS" RAW, sense prefixos.
  targets:
    - my_android
  target_data:
    my_android:
      tts_text: "El carter es a la porta."
      media_stream: alarm_stream_max
      clickAction: /lovelace/main
```

#### 4. Multi destinacio
Com enviar a multiples destinacions.

```yaml
action: universal_notifier.send
data:
  message: La rentadora ha acabat el cicle.
  title: Avís Rentadora
  priority: true
  targets:
    - google_home
    - telegram_bob
    - mobile_bob
  target_data:
    google_home:
      entity_id: media_player.cuina
      volume: 0.3
    mobile_bob:
      image: "https://www.home-assistant.io/images/default-social.png"
      color: red
      channel: "rentadora-alert"
    telegram_bob:
      type: photo
      url: "https://www.home-assistant.io/images/default-social.png"
```

</details>

## 🪲 Resolucio de Problemes
<details>
  <summary>Fes clic per expandir</summary>
  
Per al debug, afegeix a *configuration.yaml*:

```yaml
logger:
  default: info
  logs:
    custom_components.universal_notifier: debug
```

</details>
