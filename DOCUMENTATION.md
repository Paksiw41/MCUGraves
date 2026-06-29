# MCUGraves Documentation

MCUGraves is a Paper/Spigot plugin that stores player inventory in graves on death, protects graves, and optionally charges to claim items with Vault economy.

## Requirements
- Server: Paper/Spigot 1.21+ (api-version is 1.21)
- Java: 21
- Optional dependencies:
  - Vault (for economy features)
  - LuckPerms (permission checks are supported if installed)
  - Protection plugins (GriefPrevention, WorldGuard, Lands, Towny, ProtectionStones)
  - ItemsAdder (custom items can be excluded from graves; custom blocks/furniture can be used as grave markers)
  - Nexo (custom items can be excluded from graves; custom blocks/furniture can be used as grave markers)

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
- /grave tp (opens a GUI listing your active graves; click one to teleport there. Charges grave-tp.cost if enabled. No grave id to type.)
- /grave browse (opens a GUI to recover your own expired graves for a fee)
- /grave shop (opens grave shop; economy must be enabled)

### Admin Commands
- /grave reload (reloads config and refreshes holograms)
- /grave cleanup (removes orphan holograms)
- /grave clear (removes all graves)
- /grave logs (opens an audit GUI of every expired/archived grave: player heads → contents, death info, coords teleport, and recovery cost)

## Permissions
### Player Permissions
- mcugrave.claim (claim graves)
- mcugrave.shop (open grave shop)
- mcugrave.tp (open the /grave tp GUI and teleport to your graves)
- mcugrave.toggle (open grave settings GUI)
- mcugrave.browse (open the /grave browse expired-grave recovery GUI)

### Admin Permissions
- mcugrave.reload (reload config)
- mcugrave.admin (admin override on graves; includes clear and cleanup)
- mcugrave.logs (open the /grave logs audit GUI; default op)
- mcugrave.effect.soul (select the Soul Wisp claim effect)

### Legacy Permissions (Compatibility)
- economygraves.claim
- economygraves.shop
- economygraves.tp
- economygraves.toggle
- economygraves.reload
- economygraves.admin

## Licensing
MCUGraves requires a valid, IP-whitelisted license to enable. Set your key at the top of config.yml under `license.key`, whitelist your server IP on the Paksiw license bot, and restart. Validation happens on startup against the bot (slug `mcugrave`); once activated, an `activation.json` cache lets the server start offline. The license is re-checked every 7 days; a failed check disables the plugin.

## Configuration Files
Settings are split across several files in the plugin's data folder for easier editing:
- config.yml — core gameplay settings
- grave-shop-gui.yml — the /grave shop GUI layout (grave-shop.* keys)
- grave-toggle-gui.yml — the /grave toggle GUI and claim-effects GUI layouts (grave-toggle.* and claim-effects.* keys)
- lang/messages.yml — all player-facing messages (messages.* keys)

Each file is version-stamped (config-version) and auto-migrated: missing keys are added on startup without overwriting your edits. Existing installs that still keep these sections in config.yml continue to work — the plugin falls back to config.yml when a key is absent from the new files.

## Configuration Highlights
Key config.yml sections:
- economy.enabled: enable/disable economy features
- grave-expiry.enabled and grave-expiry.time-seconds: auto-expire graves
- grave-block: the block type used for graves. Accepts ANY placeable Material, including functional/decorative blocks such as SKELETON_SKULL, WITHER_SKELETON_SKULL, ZOMBIE_HEAD, CREEPER_HEAD, PIGLIN_HEAD, DRAGON_HEAD. To use a player head, type `player-head` (a shortcut for PLAYER_HEAD that shows the grave owner's skin).
- action-bar.enabled: show/hide the active-grave countdown action bar. When disabled, surface the same info via PlaceholderAPI instead (see Placeholders → PlaceholderAPI).
- grave-archive.enabled / grave-archive.browse.* / grave-archive.logs.*: archive expired graves so /grave browse (owner recovery) and /grave logs (admin audit) can access them. Recovery price = base-cost × grave-archive.browse.price-multiplier. Times shown in Philippine time (Asia/Manila).
- graves.max-graves and max-active-graves (legacy): limit active graves
- grave-limit-behavior: DELETE_OLDEST or PREVENT_NEW
- grave-protection.*: explosion protection and owner invulnerability
- grave-compass.*: compass settings (shop GUI layout is in grave-shop-gui.yml)
- grave-tp.enabled: enable /grave tp for players (opens a GUI of their active graves; clicking one teleports there)
- grave-tp.cost.enabled and grave-tp.cost.price: optionally charge a flat fee (via Vault) each time a player teleports to a grave
- protection.*: claim hooks and spawn location behavior
- itemsadder.ignore-custom-items: ignore custom items in graves
- itemsadder.grave-block: ItemsAdder custom block/furniture ID for grave markers (overrides grave-block when set and ItemsAdder is installed)
- itemsadder.furniture-hologram-height: hologram height override for ItemsAdder furniture graves
- nexo.ignore-custom-items: ignore Nexo custom items in graves
- nexo.grave-block: Nexo custom block/furniture ID for grave markers (overrides grave-block when set and Nexo is installed; ItemsAdder takes priority if both are set)
- nexo.furniture-hologram-height: hologram height override for Nexo furniture graves

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

### PlaceholderAPI (requires PlaceholderAPI installed)
When PlaceholderAPI is installed, MCUGraves registers an expansion with the identifier `mcugraves`. Use these in any PlaceholderAPI-supported plugin (scoreboard, tab, chat). They are especially useful when `action-bar.enabled` is set to false, so you can show the grave countdown wherever you like.
- %mcugraves_time_left% (formatted countdown — H:MM:SS / MM:SS — of the soonest-expiring grave; empty if none)
- %mcugraves_time_left_seconds% (raw seconds remaining; 0 if none)
- %mcugraves_cost% (the player's current claim cost, formatted)
- %mcugraves_grave_count% (number of active/unclaimed graves the player owns)
- %mcugraves_claim_count% (the player's successful claim count)
- %mcugraves_has_grave% (true/false — whether the player has any active grave)

## Notes
- If economy is enabled and Vault is not available, the plugin disables itself on startup.
- Protection hooks are only active when protection.enabled is true.
- Graves are skipped in disabled-worlds.
- ItemsAdder and Nexo custom items can be excluded from graves to avoid duplication.
- ItemsAdder/Nexo custom blocks and furniture can be used as the grave marker. The plugin auto-detects whether the configured id is a block or furniture, cancels the plugin's own break event so the marker can never be dropped, and routes the break through the normal claim flow.
