## Features

The main feature of this plugin is to automatically fill turrets with weapons, attachments and ammo when deployed, using configurable loadouts.

- Default loadouts can be assigned to players via permissions. For example, one player can have a default loadout with an ak47, while another player can have a default loadout with an m249.
- Players can define custom loadouts for their personal use, with restrictions based on permissions. For example, one player can save only pistols with standard ammo in loadouts, while another player can save any weapon with any attachments and ammo they want.
- Auto-filled turrets can be locked to prevent players from obtaining free items.

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
- `turretloadouts.manage` -- Allows the player to use `tl`, `tl help|list`, and `tl <loadout name>`.
- `turretloadouts.manage.custom` -- Allows the player to manage custom loadouts using `tl save|update|rename|delete`, according to their allowed loadout ruleset. Also allows the commands from the `turretloadouts.manage` permission.

### Default loadouts

You can assign players default loadouts using permissions of the format `turretloadouts.default.<name>`, where `<name>` should be replaced with the name of a default loadout in the plugin configuration. For example, if you have a default loadout named `ak47` in the plugin configuration, you may grant the `turretloadouts.default.ak47` permission to assign that loadout to a player or group. If a player has access to multiple default loadouts, only the last one in the list that they have access to will apply.

A default loadout will automatically be active unless the player has used the `tl <loadout name>` command to deactivate it or to activate a custom loadout. If you want to force players to use their default loadout always, simply **do not** give them the `turretloadouts.manage` or `turretloadouts.manage.custom` perimssions.

Note: Default loadouts ignore rulesets.

### Custom loadout rulesets

By default, players with the `turretloadouts.manage.custom` permission will **not** be able to save any custom loadouts. You must grant them a ruleset permission that dictates which weapons, attachments and ammo that they may save in loadouts.

Each ruleset defined in the plugin configuration will automatically generate a corresponding permission of the format `turretloadouts.ruleset.<name>` when the plugin loads. Granting one of those permissions to a player will assign that ruleset to them. If a player has permissions to multiple rulesets, only the last one in the configuration that they have permission to will apply (i.e., rulesets cannot be combined).

Note: Updating rulesets or changing a player's ruleset will automatically adjust existing custom loadouts when the player uses them.

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
  ]
}
```

- `LockAutoFilledTurrets` (`true` or `false`) -- While `true`, turrets that are automatically filled will be locked so that players cannot add or remove the weapon, attachments or ammo.
  - Locked turrets will not drop items when destroyed.
  - Locked turrets can still be picked up.
  - When a locked turret runs out of ammo, a player can power it down to automatically remove the turret's weapon and unlock its inventory. This allows the player to use the turret like normal (otherwise it would be useless).
- `MaxLoadoutsPerPlayer` -- The maximum number of custom loadouts that each player can have at once.
- `DefaultLoadouts` -- List of default loadouts.
  - `Name` -- Name that will be used to automatically generate a permission of the format `turretloadouts.default.<name>`.
  - `Weapon` -- Short name of the weapon item to add to the turret.
  - `Skin` -- Numeric skin id that should be applied to the weapon. Exclude this option or set to `0` for the default skin.
  - `Attachments` -- List of attachment item short names to add to the weapon.
  - `Ammo` (`name`, `amount`) -- Ammo type (short name) and amount to load into the weapon initially. You can safely set this to higher than the weapon can hold and the weapon will not be given extra ammo.
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
    - If this option is used, only the ammo types specified will be allowed. Unspecified ammo types will max at 0.

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
