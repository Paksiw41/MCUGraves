# MCUGraves Documentation

MCUGraves is a Paper/Spigot plugin that stores player inventory in graves on death, protects graves, and optionally charges to claim items with Vault economy.

## Requirements
- Server: Paper/Spigot 1.21+ (api-version is 1.21)
- Java: 21
- Optional dependencies:
  - Vault (for economy features)
  - LuckPerms (permission checks are supported if installed)
  - Protection plugins (GriefPrevention, WorldGuard, Lands, Towny, ProtectionStones)
  - ItemsAdder (custom items can be excluded from graves)

## Installation
1. Build or download the jar.
2. Place the jar in your server's plugins folder.
3. Start the server once to generate config files.
4. Edit settings in config.yml and restart or run /grave reload.

## How It Works (Player Flow)
- On death, a grave is placed at (or near) the death location.
- Players right-click the grave to claim their items.
- If economy is enabled, the claim can cost money based on claim count.
- Graves can expire if grave-expiry is enabled.
- Optional grave compass points to a player's active grave.

## Commands
### Player Commands
- /grave (opens the Grave Settings GUI)
- /grave toggle (open Grave Settings GUI)
- /grave cost (shows claim count and claim cost)
- /grave list (lists active graves)
- /grave tp [id] (teleport to your grave; lists graves if you have more than one)
- /grave shop (opens grave shop; economy must be enabled)

### Admin Commands
- /grave reload (reloads config and refreshes holograms)
- /grave cleanup (removes orphan holograms)
- /grave clear (removes all graves)

## Permissions
### Player Permissions
- mcugrave.claim (claim graves)
- mcugrave.shop (open grave shop)
- mcugrave.tp (teleport to your grave)
- mcugrave.toggle (open grave settings GUI)

### Admin Permissions
- mcugrave.reload (reload config)
- mcugrave.admin (admin override on graves; includes clear and cleanup)
- mcugrave.effect.soul (select the Soul Wisp claim effect)

### Legacy Permissions (Compatibility)
- economygraves.claim
- economygraves.shop
- economygraves.tp
- economygraves.toggle
- economygraves.reload
- economygraves.admin

## Configuration Highlights
All options live in [src/main/resources/config.yml](src/main/resources/config.yml). Key sections:
- economy.enabled: enable/disable economy features
- grave-expiry.enabled and grave-expiry.time-seconds: auto-expire graves
- grave-block: the block type used for graves
- graves.max-graves and max-active-graves (legacy): limit active graves
- grave-limit-behavior: DELETE_OLDEST or PREVENT_NEW
- grave-protection.*: explosion protection and owner invulnerability
- grave-shop.* and grave-compass.*: shop and compass settings
- grave-toggle.* and claim-effects.*: GUI and claim effect settings
- grave-tp.enabled: enable /grave tp for players
- protection.*: claim hooks and spawn location behavior
- itemsadder.ignore-custom-items: ignore custom items in graves

## Placeholders
### Messages (config messages.*)
- %claim_count% (successful claim count)
- %death_count% (alias of claim count)
- %cost% (formatted claim cost)
- %balance% (player balance)
- %count% (grave count)
- %time_left% (time left on the soonest expiring grave)
- %effect% (selected claim effect name)

### Holograms (hologram.lines)
- %player% (grave owner name)
- %death_cause% (death message)
- %time% (formatted time of death)
- %cost% (formatted claim cost)
- %claim_count% (successful claim count)
- %death_count% (alias of claim count)

### Grave Shop / Compass Items
- %price% (shop price for compass)
- %player% (player name)
- %grave_id% (grave UUID)
- %time_left% (compass time left if expiry is used)
- %reason% (inactive reason: expired, claimed, owner-mismatch)

### GUIs
- %current% (current claim effect in Grave Settings GUI)
- %selected% (selected status in Claim Effects GUI)

## Notes
- If economy is enabled and Vault is not available, the plugin disables itself on startup.
- Protection hooks are only active when protection.enabled is true.
- Graves are skipped in disabled-worlds.
- ItemsAdder custom items can be excluded from graves to avoid duplication.
