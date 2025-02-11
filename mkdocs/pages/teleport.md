---
status: new
---

# Teleport System

![Teleport Menu](../assets/tpmenu.png){: style="height:150px;width:auto" align=right}

A never seen before way to let users teleport! This script can either be used by a track author **or** by a server owner to make spawns available in both single and multiplayer! The user does not need to download or set up anything. It also comes with a nice GUI that shows locations aswell as the available and blocked spawns within that location.


## How it works

The track owner (or whoever really) can set up teleports and save the configuration online. The player downloads a copy automatically when playing on a track or server that has this script included. Once a player drives into a "trigger", the teleport menu will open up, allowing the user to visually inspect and click on the location they desire to go.

The teleports are always fetched [from github](https://github.com/Pivot-Point-Labs/script-configs/tree/main), unless the user has no internet acces - in which case the last downloaded configuration will be used.


## Using this script

=== "For track makers"

    Simply [download the script](https://github.com/Pivot-Point-Labs/scripts/blob/main/teleport-system/teleport.lua), place it into your `extension` folder (or a subfolder) and add this to your `ext_config.ini`:

    ```ini
    [SCRIPT_...]
    SCRIPT='<path_to_lua_from_extensions_folder>/teleport.lua'
    ```

    Then add the following at the top of your `surfaces.ini`
    ```ini
    [_SCRIPTING_PHYSICS]
    ALLOW_TRACK_SCRIPTS=1
    ```

    And add or change `WAV_PITCH` to `WAV_PITCH=extended-0` on `[SURFACE_0]`

    Example:

    ```ini
    [_SCRIPTING_PHYSICS]
    ALLOW_TRACK_SCRIPTS=1

    [SURFACE_0]
    ...
    WAV_PITCH=extended-0
    ...
    ```

=== "For server owners"

    Server owners can use the [raw GIT url](https://raw.githubusercontent.com/Pivot-Point-Labs/scripts/refs/heads/main/teleport-system/teleport.lua) to load scripts directly from github, without downloading anything. Add this to your [CSP extra options](https://github.com/ac-custom-shaders-patch/acc-extension-config/wiki/Misc-%E2%80%93-Server-extra-options) in Content Manager (if you use this to configure your server):

    !!! info
        Keep in mind that a config has to be [created and uploaded](https://github.com/Pivot-Point-Labs/script-configs/tree/main/online/tracks) for the server script to work!

    ```ini
    [SCRIPT_...]
    SCRIPT = 'https://raw.githubusercontent.com/Pivot-Point-Labs/scripts/refs/heads/main/teleport-system/teleport.lua'
    ```

    Thats it, you're all set up!

=== "For players"

    Players usually don’t need to do anything! If the track author has included the script with the map—or if the server you are playing on is running this script—teleports will "just work."

    However, if you want to create your own teleports for maps that don’t have them included, the setup is pretty much the same as for track authors. Keep in mind that modifying a track yourself may prevent you from playing online.

    

## Creating teleports & debugging

Instead of loading a config file from github, you can create your own `teleports.json` next to your `ext_config.ini`, which will be loaded instead. 
I suggest you take an already existing config and modify it to your needs. You can display debugging visuals by modifying your ext_config entry like this:

```ini
[SCRIPT_...]
debug=true ; this adds debugging cubes for teleport-triggers and spawns
SCRIPT='teleport.lua'
```

!!! info 
    Debugging works client **and/or** server side; however, the teleports.json file will only be loaded in singleplayer.

I also suggest opening the Lua Debug app and searching for something along the lines of "Track Script". It will show you the cars current position and your camera heading, which makes it a lot easier to copy values into the json file.

??? example "An example teleports.json could look like this (click to open):"

    ```json title="teleports.json"
    {
        "triggers": [
            [17.09,0,-13.10],
            [21.06,0,-13.07],
            [25.17,0,-13.10],
            [29.13,0,-13.05]
        ],
        "locations": {
            "Location 1": {
                "spawns": [
                    {
                        "position": [25.18,0,-17.11],
                        "heading": [0.36,0.00,-0.93]
                    },
                    {
                        "position": [25.13,-0,-21.15],
                        "heading": [0.36,0.00,-0.93]
                    },
                    {
                        "position": [25.10,0,-25.18],
                        "heading": [0.36,0.00,-0.93]
                    }
                ],
                "img": "https://files.catbox.moe/y4mlg8.png"
            },
            "Location 2": {
                "spawns": [
                    {
                        "position": [21.09,0,-17.10],
                        "heading": [0.36,0.00,-0.93]
                    },
                    {
                        "position": [21.09,0,-21.10],
                        "heading": [0.36,0.00,-0.93]
                    }
                ],
                "img": "https://files.catbox.moe/9vdiu1.png"
            },
            "Location 3": {
                "spawns": [
                    {
                        "position": [17.06,0,-17.04],
                        "heading": [-0.978682, 0.201236, -0.0410538]
                    }
                ],
                "img": "https://files.catbox.moe/00tlo7.png"
            }
        }
    }
    ```

- The `triggers` array consists of coordinate points that serve as the activation point of the teleport menu (drive through and the menu appears).
- The `locations` object (Labeled in the example as "Location 1", "Location 2", ...,) defines each location on the track for players to teleport to. Each location object is defined by two things:
    - Image: A URL pointing to an image used as preview.
    - Spawns:  defines the coordinates for spawns. Compirised of:
        - `Position`: the exact coordinates to place a spawnpoint within a location
        - `Heading`: a direction vector that indicated where the car is facing after spawning
    

## Submitting your config

After you created a config and validated it in singleplayer, you can create a [GitHub issue](https://github.com/Pivot-Point-Labs/script-configs/issues/new?title=[Teleport Config] request&body=Track folder: YOUR_TRACK_FOLDER%0A%0A%60%60%60json%0APlease put your json config within these quotes%0A%60%60%60){:target="_blank"}, a [pull request](https://github.com/Pivot-Point-Labs/script-configs/pulls){:target="_blank"} or you text me on discord (please don't, just figure out how to use GitHub).

Once a config is accepted and added to the repository, it will be available to everyone.

