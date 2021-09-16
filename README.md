## Features

- Automatically adds weapons, attachments and ammo to auto turrets when deployed
- Automatically adds ammo to sam sites, flame turrets and shotgun traps when deployed
- Allows assigning default loadouts via permissions
- Allows players to define multiple personal loadouts for auto turrets, with restrictions based on permission
- Provides weapons, attachments and ammo for free
- Optionally locks auto-filled turrets to prevent abusing free items
- Allows automatically deploying auto turrets in peacekeeper mode

## Commands

- `tl` -- Display your active turret loadout, including weapon, loaded ammo, attachments and reserve ammo.
- `tl help` -- Display help information about the commands you are allowed to use.
- `tl list` -- List your loadouts, including default and custom.
- `tl <loadout name>` -- Activate or deactivate the specified loadout. The active loadout applies when you deploy a turret. Having no active loadout will allow you to deploy a turret without adding any items to it.
- `tl save <name>` -- Save a custom loadout with the turret you are aiming at.
- `tl update <name>` -- Update an existing custom loadout with the turret you are aiming at.
- `tl rename <name> <new name>` -- Rename a custom loadout.
- `tl delete <name>` -- Delete a custom loadout.

## Permissions

- `turretloadouts.autoauth` -- For players with this permission, deploying a turret will automatically authorize the player to it.
- `turretloadouts.autotoggle` -- For players with this permission, deploying a turret will automatically power it on, but only if it was filled with a weapon and ammo.
- `turretloadouts.autotoggle.samsite` -- For players with this permission, deploying a sam site will automatically power it on, but only if filled with some ammo.
- `turretloadouts.manage` -- Allows the player to use `tl`, `tl help|list`, and `tl <loadout name>`.
- `turretloadouts.manage.custom` -- Allows the player to manage custom loadouts using `tl save|update|rename|delete`, according to their allowed loadout ruleset. Also allows the commands from the `turretloadouts.manage` permission.

### Default auto turret loadouts

- `turretloadouts.default.<name>` -- Assigns the auto turret loadout with the specified name to the player, causing any auto turrets they deploy to be filled with that loadout.
   - Default loadouts are defined in the plugin configuration under `DefaultLoadouts`. Each one generates a separate permission.
   - Granting multiple default loadouts to a player will cause only the last one to apply, based on the order in the plugin configuration.
   - After assigning a default loadout to a player, that loadout will automatically be active, unless the player has used the `tl <loadout name>` command to deactivate it or to activate a custom loadout. If you want to force players to use their default loadout always, simply **do not** give them the `turretloadouts.manage` or `turretloadouts.manage.custom` perimssions.
   - Default loadouts ignore rulesets and ammo stack size limits.

### Sam sites, flame turrets and shotgun traps

This plugin does **not** allow players to define custom loadouts for sam sites, flame turrets and shotgun traps because the only thing worth customizing about them is the amount of ammo, and ammo is currently free with this plugin.

- `turretloadouts.samsite.default.<name>` -- Assigns the sam site loadout with the specified name to the player, causing any sam sites they deploy to have the configured ammo.
  - Sam site loadouts are defined in the plugin configuration under `DefaultSamSiteLoadouts`. Each one generates a separate permission.
  - Granting multiple default sam site loadouts to a player will cause only the last one to apply, based on the order in the plugin configuration.
- `turretloadouts.flameturret.default.<name>` -- Assigns the flame turret loadout with the specified name to the player, causing any flame turrets they deploy to have the configured ammo.
  - Flame turret loadouts are defined in the plugin configuration under `DefaultFlameTurretLoadouts`. Each one generates a separate permission.
  - Granting multiple default flame turret loadouts to a player will cause only the last one to apply, based on the order in the plugin configuration.
- `turretloadouts.shotguntrap.default.<name>` -- Assigns the shotgun trap loadout with the specified name to the player, causing any shotgun traps they deploy to have the configured ammo.
  - Shotgun trap loadouts are defined in the plugin configuration under `DefaultShotgunTrapLoadouts`. Each one generates a separate permission.
  - Granting multiple default shotgun trap loadouts to a player will cause only the last one to apply, based on the order in the plugin configuration.

### Custom loadout rulesets

- `turretloadouts.ruleset.<name>` -- Assigns the ruleset with the specified name to the player, determining which weapons, attachments, ammo types, and ammo quantity that they can save in custom loadouts.
  - Rulesets are defined in the plugin configuration under `LoadoutRulesets`. Each one generates a separate permission.
  - You **must** grant players a ruleset permission to allow them to save custom loadouts, in addition to granting the `turretloadouts.manage.custom` permission.
  - Granting multiple rulesets to a player will cause only the last one to apply, based on the order in the plugin configuration.
  - Updating rulesets or changing a player's ruleset will automatically adjust existing custom loadouts when the player uses them.

## Configuration

Default configuration:

```json
{
  "LockAutoFilledTurrets": false,
  "MaxLoadoutsPerPlayer": 10,
  "DefaultLoadouts": [
    {
      "Name": "ak47",
      "Weapon": "rifle.ak",
      "Skin": 885146172,
      "Ammo": {
        "Name": "ammo.rifle",
        "Amount": 30
      },
      "ReserveAmmo": [
        {
          "Name": "ammo.rifle",
          "Amount": 128
        },
        {
          "Name": "ammo.rifle",
          "Amount": 128
        }
      ]
    },
    {
      "Name": "m249",
      "Weapon": "lmg.m249",
      "Skin": 1831294069,
      "Attachments": [
        "weapon.mod.lasersight",
        "weapon.mod.silencer"
      ],
      "Ammo": {
        "Name": "ammo.rifle.explosive",
        "Amount": 100
      },
      "ReserveAmmo": [
        {
          "Name": "ammo.rifle.incendiary",
          "Amount": 128
        },
        {
          "Name": "ammo.rifle.hv",
          "Amount": 128
        }
      ]
    }
  ],
  "LoadoutRulesets": [
    {
      "Name": "onlypistols",
      "AllowedWeapons": [
        "pistol.eoka",
        "pistol.m92",
        "pistol.nailgun",
        "pistol.python",
        "pistol.revolver",
        "pistol.semiauto",
        "pistol.water"
      ],
      "AllowedAmmo": {
        "ammo.pistol": 600,
        "ammo.pistol.hv": 400,
        "ammo.pistol.fire": 200
      }
    },
    {
      "Name": "norifles",
      "DisallowedWeapons": [
        "rifle.ak",
        "rifle.bolt",
        "rifle.l96",
        "rifle.lr300",
        "rifle.m39",
        "rifle.semiauto",
        "lmg.m249"
      ]
    },
    {
      "Name": "unlimited"
    }
  ],
  "DefaultSamSiteLoadouts": [
    {
      "Name": "fullammo",
      "ReserveAmmo": [
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        },
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        },
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        },
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        },
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        },
        {
          "Name": "ammo.rocket.sam",
          "Amount": 128
        }
      ]
    }
  ],
  "DefaultFlameTurretLoadouts": [
    {
      "Name": "fullammo",
      "ReserveAmmo": [
        {
          "Name": "lowgradefuel",
          "Amount": 500
        }
      ]
    }
  ],
  "DefaultShotgunTrapLoadouts": [
    {
      "Name": "fullammo",
      "ReserveAmmo": [
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        },
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        },
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        },
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        },
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        },
        {
          "Name": "ammo.handmade.shell",
          "Amount": 128
        }
      ]
    }
  ]
}
```

- `LockAutoFilledTurrets` (`true` or `false`) -- While `true`, turrets that are automatically filled will be locked so that players cannot add or remove the weapon, attachments or ammo.
  - Locked turrets will **not** drop items when destroyed or removed. Compatible with the Remover Tool plugin.
  - Locked turrets can still be picked up.
  - When a locked turret runs out of ammo, a player can power it down to automatically remove the turret's weapon and unlock its inventory. This allows the player to use the turret like normal (otherwise it would be useless).
  - When a locked sam site, flame turret or shotgun trap runs out of ammo, it will have to be picked up and placed again. If this becomes an issue, simply set the ammo amounts to ridiculously high values so that they never run out.
- `MaxLoadoutsPerPlayer` -- The maximum number of custom loadouts that each player can have at once.
- `DefaultLoadouts` -- List of default loadouts for auto turrets.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.default.<name>`.
  - `Weapon` -- Short name of the weapon item to add to the turret.
  - `Skin` -- Numeric skin id that should be applied to the weapon. Exclude this option or set to `0` for the default skin.
  - `Peacekeeper` (`true` or `false`) -- While `true`, the turret will be placed in peacekeeper mode.
  - `Attachments` -- List of attachment item short names to add to the weapon.
  - `Ammo` (`Name`, `Amount`) -- Ammo type (short name) and amount to load into the weapon initially. You can safely set this to higher than the weapon can hold and the weapon will not be given extra ammo.
  - `ReserveAmmo` -- List of ammo types (short names) and amounts to add to the turret inventory.
- `LoadoutRulesets` -- List of rulesets that control which weapons, attachments and ammo players can save in their custom loadouts.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.ruleset.<name>`.
  - `AllowedWeapons` -- List of weapon item short names to allow in custom loadouts. Overrides `DisallowedWeapons`.
    - Exclude this option to allow all weapons or to use `DisallowedWeapons` instead.
    - Setting to `[]` will allow no weapons (which is useless).
  - `DisallowedWeapons` -- List of weapon item short names to disallow in custom loadouts. Ignored if `AllowedWeapons` is present.
  - `AllowedAttachments` -- List of attachment item short names to allow in custom loadouts. Overrides `DisallowedAttachments`.
    - Exclude this option to allow all attachments, or to use `DisallowedAttachments` instead.
    - Setting to `[]` will allow no attachments.
  - `DisallowedAttachments` -- List of attachment item short names to disallow in custom loadouts. Ignored if `AllowedAttachments` is present.
  - `AllowedAmmo` -- Mapping of ammo item short names to total amounts to allow per custom loadout. This includes both the loaded ammo and reserve ammo. For example, if you allow 200 rifle ammo, a player may save a loadout with 30 ammo in the weapon and at most 170 in reserve.
    - Exclude this option to allow unlimited ammo of all types.
    - Setting to `{}` will allow no ammo.
    - Setting an ammo type to `-1` will allow unlimited ammo of that type.
    - If this option is used, only the ammo types specified will be allowed. Unspecified ammo types will max at 0.
- `DefaultSamSiteLoadouts` -- List of default loadouts for sam sites. Each loadout has the following options.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.samsite.default.<name>`.
  - `ReserveAmmo` -- List of ammo types (short names) and amounts to add to the sam site inventory.
- `DefaultFlameTurretLoadouts` -- List of default loadouts for flame turrets. Each loadout has the following options.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.flameturret.default.<name>`.
  - `ReserveAmmo` -- List of ammo types (short names) and amounts to add to the flame turret inventory.
- `DefaultShotgunTrapLoadouts` -- List of default loadouts for shotgun traps.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.shotguntrap.default.<name>`.
  - `ReserveAmmo` -- List of ammo types (short names) and amounts to add to the shotgun trap inventory.

### Example rulesets

#### No restrictions

Any weapon, any ammo, and any attachments.
```json
{
  "Name": "unlimited"
}
```

#### No attachments

Any weapon, any ammo, but no attachments.
```json
{
  "Name": "noattachments",
  "AllowedAttachments": []
}
```

#### No ammo

Any weapon, any attachments, but no ammo.
```json
{
  "Name": "noammo",
  "AllowedAmmo": {}
}
```

#### No attachments or ammo

Any weapon, but no attachments or ammo.
```json
{
  "Name": "onlyweapons",
  "AllowedAttachments": [],
  "AllowedAmmo": {}
}
```

#### No m249 or silencer

Any weapon except M249, any attachments except silencer, and any ammo.
```json
{
  "Name": "balanced",
  "DisallowedWeapons": ["lmg.m249"],
  "DisallowedAttachments": ["weapon.mod.silencer"]
}
```

## Localization

```json
{
  "Generic.RestrictedItems": "<color=#f44>Restricted items not saved:</color>",
  "Generic.DefaultLoadoutName": "Default",
  "Generic.Error.NoPermission": "You don't have permission to use this command.",
  "Generic.Error.NoTurretWeapon": "Error: That auto turret has no weapon.",
  "Generic.Error.WeaponNotAllowed": "Error: Weapon not allowed: <color=#f44>{0}</color>.",
  "Generic.Error.LoadoutNotFound": "Error: Loadout <color=#fe4>{0}</color> not found.",
  "Generic.Error.LoadoutNameLength": "Error: Loadout name may not be longer than <color=#fe4>{0}</color> characters.",
  "Generic.Error.DefaultLoadout": "Error: You cannot edit the default loadout.",
  "Generic.Header": "<size=16><color=#fa5>Turret Loadouts</color></size>",
  "Generic.FilledFromLoadout": "Filled turret with loadout: <color=#fe4>{0}</color>. Type <color=#fe4>/tl help</color> for more options.",
  "Command.Activate.Error.Syntax": "Syntax: <color=#fe4>tl <loadout name></color>",
  "Command.Activate.Success.Deactivated": "Deactivated <color=#fe4>{0}</color> loadout.",
  "Command.Default.HelpHint": "Use <color=#fe4>tl help</color> for more options.",
  "Command.Default.NoActive": "No active turret loadout.",
  "Command.Default.Active": "<size=16><color=#fa5>Active Turret Loadout</color>: {0}</size>",
  "Command.Default.Mode.AttackAll": "<color=#fe4>Mode</color>: <color=#f44>Attack All</color>",
  "Command.Default.Mode.Peacekeeper": "<color=#fe4>Mode</color>: <color=#6e6>Peacekeeper</color>",
  "Command.Default.Weapon": "<color=#fe4>Weapon</color>: {0}{1}",
  "Command.Default.Attachments": "<color=#fe4>Attachments</color>:",
  "Command.Default.ReserveAmmo": "<color=#fe4>Reserve ammo</color>:",
  "Command.List.NoLoadouts": "You don't have any turret loadouts.",
  "Command.List.ToggleHint": "Use <color=#fe4>tl <loadout name></color> to activate or deactivate a loadout.",
  "Command.List.Item": "<color=#fe4>{1}</color>{0} - {2}{3}",
  "Command.List.Item.Active": " <color=#5bf>[ACTIVE]</color>",
  "Command.Save.Error.Syntax": "Syntax: <color=#fe4>tl save <name></color>",
  "Command.Save.Error.NoTurretFound": "Error: No auto turret found.",
  "Command.Save.Error.LoadoutExists": "Error: Loadout <color=#fe4>{0}</color> already exists. Use <color=#fe4>tl update {0}</color> to update it.",
  "Command.Save.Error.TooManyLoadouts": "Error: You may not have more than <color=#fe4>{0}</color> loadouts. You may delete another loadout and try again.",
  "Command.Save.Success": "Turret loadout saved as <color=#fe4>{0}</color>. Activate it with <color=#fe4>tl {0}</color>.",
  "Command.Update.Error.Syntax": "Syntax: <color=#fe4>tl update <name></color>",
  "Command.Update.Success": "Updated <color=#fe4>{0}</color> loadout.",
  "Command.Rename.Error.Syntax": "Syntax: <color=#fe4>tl rename <name> <new name></color>",
  "Command.Rename.Error.LoadoutNameTaken": "Error: Loadout name <color=#fe4>{0}</color> is already taken.",
  "Command.Rename.Success": "Renamed <color=#fe4>{0}</color> loadout to <color=#fe4>{1}</color>.",
  "Command.Delete.Error.Syntax": "Syntax: <color=#fe4>tl delete <name></color>",
  "Command.Delete.Success": "Deleted <color=#fe4>{0}</color> loadout.",
  "Command.Help.Details": "<color=#fe4>tl</color> - Show your active loadout details",
  "Command.Help.List": "<color=#fe4>tl list</color> - List turret loadouts",
  "Command.Help.Activate": "<color=#fe4>tl <loadout name></color> - Toggle whether a loadout is active",
  "Command.Help.Save": "<color=#fe4>tl save <name></color> - Save a loadout with the turret you are aiming at",
  "Command.Help.Update": "<color=#fe4>tl update <name></color> - Overwrite an existing loadout with the turret you are aiming at",
  "Command.Help.Rename": "<color=#fe4>tl rename <name> <new name></color> - Rename a loadout",
  "Command.Help.Delete": "<color=#fe4>tl delete <name></color> - Delete a loadout",
  "Abbreviation.weapon.mod.8x.scope": "16x",
  "Abbreviation.weapon.mod.flashlight": "FL",
  "Abbreviation.weapon.mod.holosight": "HS",
  "Abbreviation.weapon.mod.lasersight": "LS",
  "Abbreviation.weapon.mod.muzzleboost": "MBS",
  "Abbreviation.weapon.mod.muzzlebrake": "MBR",
  "Abbreviation.weapon.mod.silencer": "SL",
  "Abbreviation.weapon.mod.simplesight": "SS",
  "Abbreviation.weapon.mod.small.scope": "8x"
}
```
