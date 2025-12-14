# Bigfoot

![Bigfoot](assets/logo.svg)

Plugin para **RocketModFix / Unturned** enfocado en:

- Logs a Discord **sin lag** (webhooks con cola acotada y envío en background)
- Bot Discord (Discord.Net) para comandos de admin y utilidades
- **Alertas de raid** con mención al dueño (si está vinculado)
- SPY screenshot (`/spy <nombre>`) con envío estable

> Este repo incluye un paquete listo para descargar.

---

## Estructura del paquete (GitHub)

Dentro de `github-package/`:

- `plugin/Bigfoot.dll`
  - Va en `Rocket/Plugins/Bigfoot.dll`
- `libraries/*.dll`
  - Dependencias (Discord.Net, LiteDB, etc.)
  - Van en `Rocket/Libraries/`
- `Bigfoot.configuration.example.xml`
  - Plantilla de configuración (copiar y renombrar)

---

## Instalación (paso a paso)

1. **Apaga el servidor** (importante para evitar archivos bloqueados).
2. Copia el plugin:
   - `github-package/plugin/Bigfoot.dll` → `.../Rocket/Plugins/Bigfoot.dll`
3. Copia las librerías:
   - Todo lo de `github-package/libraries/` → `.../Rocket/Libraries/`
4. Crea la carpeta de config si no existe:
   - `.../Rocket/Plugins/Bigfoot/`
5. Copia la config de ejemplo y renómbrala:
   - `github-package/Bigfoot.configuration.example.xml` →
     `.../Rocket/Plugins/Bigfoot/Bigfoot.configuration.xml`
6. Edita `Bigfoot.configuration.xml` y configura tus IDs/URLs.
7. Enciende el servidor.

### Permisos que necesita el bot en Discord

En el canal donde usas el bot y donde envía archivos:

- `View Channel`
- `Send Messages`
- `Embed Links`
- `Attach Files` (obligatorio para `/spy`)
- `Manage Messages` (recomendado si quieres que borre mensajes de link/códigos)

---

## Configuración (Bigfoot.configuration.xml)

Los nombres de tags deben ser **exactos** (XML estricto). Si metes tags dentro de tags (contenido “complejo”), Rocket falla al deserializar.

Campos principales:

- `VerboseConsoleLogs` (bool): logs detallados para debug.
- **RCON/Discord bot**
  - `EnableRcon` (bool)
  - `BotToken` (string)
  - `RconChannelId` (ulong): canal de comandos
  - `LinkChannelId` (ulong): canal para códigos de vinculación
  - `AllowedAdminRoles`:
    - `RoleId` (repetible): roles con permisos admin en el bot
- **Panel de estado**
  - `StatusChannelId` (ulong)
  - `StatusMessageId` (ulong) – normalmente se deja en `0`
  - `StatusConnectUrl` (string) – opcional
- **Raid alerts**
  - `RaidAlertChannelId` (ulong)
- **Webhooks**
  - `GlobalChatWebhook`, `GroupChatWebhook`
  - `AdminCommandsWebhook`, `UserCommandsWebhook`
  - `ConnectionWebhook`, `DossierIpWebhook`
  - `PvPWebhook`, `DeathWebhook`

### Ejemplo de AllowedAdminRoles

```xml
<AllowedAdminRoles>
  <RoleId>111111111111111111</RoleId>
  <RoleId>222222222222222222</RoleId>
</AllowedAdminRoles>
```

---

## Vinculación + Alertas de raid

- Un jugador se vincula con un código (4 dígitos).
- Luego puede activar/desactivar alertas.
- Cuando detecta daño a barricadas/estructuras:
  - Se envía un embed al `RaidAlertChannelId`
  - Se **menciona (ping)** el Discord del dueño si está vinculado y con alertas activas
  - Se incluye link de Steam en el embed

Anti-spam:
- 1 alerta por dueño cada 5 minutos.

---

## Comandos

### Discord

- `/spy <nombre|steamid>`
  - Solicita screenshot al juego y lo entrega al canal.
  - Incluye embed “tipo juego” con ping/posición.

### In-game

- `/link`
  - Genera un código para vincular.

- `/alerts`
  - Activa/Desactiva alertas de raid para tu cuenta vinculada.

---

## Problemas típicos (y solución)

### “Unexpected node type Element … error in XML document”

- El XML está mal formateado.
- Copia el ejemplo y edita SOLO valores. No anides tags.

### Discord 403 “Missing Permissions”

- Falta `Attach Files` o `Send Messages` en el canal/thread.
- O el canal/categoría tiene overwrites que niegan permisos.

### Discord 413 “Too Large”

- El archivo excede el límite.
- Si ves este error, el screenshot puede exceder el límite del canal/servidor.

---

## Build desde código

Requisitos:
- .NET SDK (MSBuild)

Comandos:

- Build Release + paquete dist:
  - Ejecuta `build-release.ps1`

Salida:
- `dist/Bigfoot/` (plugin + dependencias)

---

## Hecho por

Diarka Studio

## Soporte

Discord: https://discord.gg/qPBEQyBUz7

---

