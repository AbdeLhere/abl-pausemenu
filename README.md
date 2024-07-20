# Custom Pause Menu Script for FiveM

## Overview

The Custom Pause Menu Script is designed for FiveM servers to provide a highly customizable pause menu. It integrates with various frameworks and supports a range of notification systems and sound effects. This guide will walk you through installation, configuration, usage, troubleshooting, and additional support.

## Features

- **Framework Compatibility**: Works with QBCore, NewESX, and OldESX frameworks.
- **Custom Notifications**: Multiple notification systems supported, including OX, okokNotify, BrutalNotify, and DefaultGTA.
- **Sound Effects**: Customizable sounds for menu actions (opening, closing, map interactions).
- **Item-Based Map Access**: Option to require a specific item to access the map.
- **Camera Adjustments**: Automatic camera view changes based on player status (in vehicle or walking).
- **Command and UI Customization**: Set up commands to open the menu and adjust UI settings.

## Installation

### Step-by-Step Installation

1. **Download the Script**:
   - Obtain the script files from the repository or source provided.

2. **Place the Script in Server Directory**:
   - Move the script files to your FiveM server’s resources directory. For example:
     ```plaintext
     resources/[your_resource_folder]/pausemenu/
     ```

3. **Add to Server Configuration**:
   - Open your `server.cfg` file.
   - Add the following line to ensure the script is loaded and started:
     ```plaintext
     ensure pausemenu
     ```

4. **Restart the Server**:
   - Restart your FiveM server to apply the changes.

## Configuration

### Editing the Configuration File

1. **Open `config.lua`**:
   - Navigate to the `config.lua` file located in the script’s directory.

2. **Adjust the Configuration Parameters**:
   - Modify the configuration values according to your server's requirements.

#### Example Configuration

```lua
Config = {}

Config.System = {
    Debug = {
        UseDebug = 'no', -- Enable debug prints in the script. Set to 'yes' to activate.
    },
    Framework = {
        Core = 'QBCore',        -- The core framework being used. Options: [QBCore / NewESX / OldESX]
        FolderName = 'qb-core', -- The folder name where the core object is located. Options: [es_extended / getSharedObject / qb-core / ...]
    },
    Notify = {
        NotifyType = 'OX', -- Type of notification system to use. Options: [DefaultQB / DefaultESX / OX / okokNotify / BrutalNotify / DefaultGTA / None]
        NotifyTime = 1500, -- Duration of notifications in milliseconds. This setting does not apply to NotifyType = "DefaultGTA".
        UseFor = {
            -- Note! This setting does not apply to NotifyType =  "DefaultGTA". Emojis can be added like "❌" or "✅" to the notifications.
            WhenUIMenuOpen = { string = 'no', type = 'success' },    -- Notification when the UI menu is opened. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
            WhenUIMenuClose = { string = 'no', type = 'success' },   -- Notification when the UI menu is closed. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
            WhenSettingsOpen = { string = 'no', type = 'success' },  -- Notification when the settings menu is opened. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
            WhenMapOpen = { string = 'yes', type = 'success' },      -- Notification when the map is opened. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
            WhenNoMapItem = { string = 'yes', type = 'error' },      -- Notification when the map item is not found. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
            WhenNoTabletItem = { string = 'yes', type = 'success' }, -- Notification when the tablet item is not found. Options: [ 'yes' / 'no'], Notification type: [ 'success', 'error', 'primary' ]
        }
    },
}

Config.Options = {
    UIMenu = {
        Sounds = {
            PopUpSoundOpen = 'no',  -- Use a custom sound when the UI opens. Options: [ 'yes' / 'no']
            PopUpSoundClose = 'no', -- Use a custom sound when the UI closes. Options: [ 'yes' / 'no']
            MapPaperSound = 'yes',  -- Use a custom sound when the map is opened. Options: [ 'yes' / 'no']
            SettingsSound = 'no'    -- Use a custom sound when the settings menu opens. Options: [ 'yes' / 'no']
        },
        Command = {
            UseCommand = 'yes',        -- Enable or disable the use of a command to open the menu. Options: [ 'yes' / 'no']
            CommandName = 'PauseMenu', -- The command name to open the menu. Default is 'PauseMenu'.
        },
        Keybind = { 
            Key = 200
        }
    },
    MapItem = {
        NeedItemForMap = 'yes',  -- Specify if a specific item is required to open the map. Options: [ 'yes' / 'no']
        ItemName = 'tablet',     -- The name of the item required to open the map (e.g., 'tablet').
        CloseUIIfNoItem = 'yes', -- Close the UI if the required item is not found. Options: [ 'yes' / 'no']
    },
    Camera = {
        InVehicle = 'yes', -- Change the camera to first-person view when opening the pause menu while in a vehicle. Options: [ 'yes' / 'no']
        Walking = 'yes'    -- Change the camera to first-person view when opening the pause menu while on foot. Options: [ 'yes' / 'no']
    },
    OnNUIStart = function() -- Add any additional functionality you want to execute when the menu opens.
    end,
    OnNUIClose = function() -- Add any additional functionality you want to execute when the menu closes.
    end
}

Config.Lang = {
    ----------- Others
    QuitMessage = 'You have left! We hope you enjoyed and liked our custom server!',
    ----------- Notify
    WhenUIMenuOpen = 'Menu Opened!',
    WhenUIMenuClose = 'Menu Closed!',
    WhenSettingsOpen = 'Settings Opened!',
    WhenMapOpen = 'Map Opened! Find your destinations.',
    WhenNoTabletItem = 'You do not have the Tablet item!',
    NoMapItem = 'You do not have the Map item!',
}
```
## FAQ

### Why are notifications not appearing in the game?

- **Notification Type**: Ensure that `Config.System.Notify.NotifyType` is set to a valid notification system that is installed on your server.
- **Notification System**: Confirm that the notification system’s export functions are correctly implemented and accessible.
- **`DefaultGTA` Notifications**: For `DefaultGTA`, notifications do not have a duration. Ensure other notification types are correctly configured if not using `DefaultGTA`.

### The map item requirement isn’t working. What should I check?

- **Item Name**: Verify that `Config.Options.MapItem.ItemName` matches the exact name used in your inventory system.
- **Inventory Check**: Ensure that the `CheckMapItem` function correctly checks for the item in the player’s inventory and that the item exists.

### How do I contribute to the script or report bugs?

- **Contributing**: Fork the repository, make your changes, and submit a pull request with a clear description of the modifications.
- **Reporting Bugs**: Open an issue on the GitHub repository with detailed information about the bug, including steps to reproduce, error messages, and any relevant configuration.

## Usage

### Opening and Closing the Menu

- **Command**: Use the command specified in `Config.Options.UIMenu.Command.CommandName` (default: `/PauseMenu`) to open the menu.
- **Control Key**: By default, the menu can be toggled with the Escape `ESC` key. Ensure that the key binding does not conflict with other controls.

### Notifications and Sounds

- **Notifications**: Configure the types of notifications and their settings in the `Config` table. For `DefaultGTA`, note that notification duration settings will not apply.
- **Sounds**: Adjust sound settings for various menu actions in the `Config.Options.UIMenu.Sounds` table.

### Item-Based Map Access

- If `Config.Options.MapItem.NeedItemForMap` is set to `'yes'`, players must have the specified item in their inventory to access the map. Ensure the item name matches exactly with the inventory item used in your server.

### Camera Adjustments

- The script can automatically adjust the camera view when the menu is opened based on whether the player is in a vehicle or walking. Configure this in `Config.Options.Camera`.
