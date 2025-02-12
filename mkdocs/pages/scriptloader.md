---
status: new
---

# Scriptloader

This is a very simple script that just serves as a loader for scripts hosted online. The CSP API does not allow track scripts to load scripts via URL directly (unlike apps or server scripts, where you can simply pass a URL instead of a path), however we can go around this by loading the script (that we actually want) from within this script loader.

The advantage of this over a track-script directly built into the map is simply that it can be updated "on the fly", without the track author needing to ship a new version of their track with the updated script.

!!! info
    Since this loader script takes in a URL, it only works if the player actually has an internet connection! Scripts are not stored locally.

!!! danger
    Loading scripts from an online source can be dangerous. The user will never see the downloaded source and the code it downloads is just run as-is. While CSP has a lot of restrictions on what a track script is even allowed to do, a bad actor could always somehow try to take advantage of this.


## Usage

[Download the scriptloader lua from github](https://github.com/Pivot-Point-Labs/scripts/blob/main/scriptloader/scriptloader.lua), place it into your `extension` folder and add this to your `ext_config.ini`

```ini
[SCRIPT_...]
url='https://some.url.to/a.script'
SCRIPT='scriptloader.lua'
```

You can add this ini config as many times as you want with different URLs.

## Important

### Script Modifications and Function Naming

Externally loaded scripts can not execute their own `script.update` or `script.update3D` functions. The way this is solved is by returning the functions itself so that this loader script can call them. This is a really hacky workaround but it seems to work just fine. Scripts must be changed to return a table with either `script.update` and `script.update3D` or `update` and `update3D` as values. It is important that the function names used and returned **MUST** match. For example:

```lua
function script.update(dt)
end

function script.draw3D()
end

return {script.update, script.draw3D}
-- NOT return {update, draw3D}
```
or
```lua
function update(dt)
end

function draw3D()
end

return {update, draw3D}
-- NOT return {script.update, script.draw3D}
```
!!! warning
    The first value has to be `update` or `script.update`, the second value has to be `draw3D` or `script.draw3D`. You can pass `nil` as value if you dont use one or the other function.


### The automated solution

This loader script has an automated solution that may or may not work. It checks if a string along the lines of `return {script.update, script.draw3D}` exists and if not, automatically appends its own code that returns the correct table. It may fail at times because pattern matching in LUA is very limited - however, it worked fine in my testing.

To use it, add `autofix=true` to the script entry

```ini
[SCRIPT_...]
url='https://some.url.to/a.script'
autofix=true
SCRIPT='scriptloader.lua'
```