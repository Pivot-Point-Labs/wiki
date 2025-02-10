# Pedestrian System

This trackscript adds support for animated and moving pedestrians. It works by defining a list of waypoints and connecting them with each other. A waypoint is just a point in 3D space at which a pedestrian can spawn. It has some settings, like the next waypoint the pedestrian should walk to & the pedestrian model that should spawn aswell as a list of models & animations. You can read more about this in the Pedconfig section.


## CSP config

The pedestrian script has two optional config entries, mainly used for debugging. Since they are optional, they dont have to exist and would instead use default values.

```ini title="ext_config.ini"
[SCRIPT_...]
debug=false
spawnPeds=false
renderDistance=300
SCRIPT='...'
```

- `debug` Shows or hides paths and waypoints
- `spawnPeds` Shows or hides pedestrians. This is mainly used to not have peds running around all the time while you want to work on your paths and waypoints.
- `renderDistance` Simply hides the peds if their waypoint is beyond this distance.

## Pedconfig

```json
{
    "waypoints": {
        "wp1" : {"pos": [-3.36, 0, 33.49], "next": ["wp2"] , "spawn": "jody"},
        "wp2" : {"pos": [8.85, 0, 30.61], "next": ["wp3"], "spawn": ["jody", "adam"]},
        "wp3" : {"pos": [6.58, 0, 20.49], "next": ["wp4"], "spawn": false},
        "wp4" : {"pos": [-10.9, 0, 19.96], "next": ["wp1"]}
    },
    "peds":{
        "jody": { "model": "pedestrian/peds/jody.kn5", "anim": "pedestrian/peds/jody.ksanim"},
        "adam": { "model": "pedestrian/peds/adam.kn5", "anim": "pedestrian/peds/adam.ksanim"},
        "brian": { "model": "pedestrian/peds/brian.kn5", "anim": "pedestrian/peds/brian.ksanim", "speed": 2}
    }
}
```

You can add as many waypoints (or peds) as you want. You are also not restricted to a single loop (like here, waypoint 1-4) but instead you can create multiple loops and select which peds spawn on which loop. For example you can create the loop 1-4; back to 1 **and** 5-8; back to 5. You can then connect those two loops and let a ped decide to follow its loop or branch of into another. This allows a range of patterns from simple to complex. The only thing you need to keep in mind is that there has to be a loop so the ped always has somewhere to go (this will be adressed in a future update)

**Waypoint**

| Argument    | Type        |  Description                          |
| ----------- | --------    | ------------------------------------- |
| `pos`       | Array       | X,Y,Z coordinates of the waypoint     |
| `next`      | Array       | List of indexes for the pedestrian to go next. If the list has more than one item, the ped chooses one random waypoint |
| `spawn` (optional)    | String/Bool  | Either the index for pedestrian or `false` to **not** spawn a pedestrian. Remove this argument and a random ped will be chosen |


**Pedestrian**

| Argument    | Type        |  Description                          |
| ----------- | --------    | ------------------------------------- |
| `model`     | String      | The path to a pedestrian kn5 model    |
| `anim`      | String      | The path to a pedestrian kn5 animation|
| `speed` (optional)     | Number/Float | The movement speed of the pedestrian (this is does not affect the animation speed).