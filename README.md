# Discord Game SDK for Python

A Python wrapper around Discord's Game SDK.
**NOTE**: This is entirely experimental, and may not work as intended. Please report all bugs to the [GitHub issue tracker](https://github.com/DoAltPlusF4/discord-game-sdk-python/issues).

**Credit to [NathaanTFM](https://github.com/NathaanTFM) for creating the [original library](https://github.com/NathaanTFM/discord-game-sdk-python).**

## Installation

- Install the module:
  - With `PIP`:
    - Stable: `python -m pip install discordsdk`
    - Latest: `python -m pip install git+https://github.com/DoAltPlusF4/discord-sdk.git`
  - With `setup.py` (latest):
    - `git clone https://github.com/DoAltPlusF4/discord-sdk.git`
    - `cd discord-sdk`
    - `python -m setup install`
- Download [Discord Game SDK (2.5.6)](https://dl-game-sdk.discordapp.net/2.5.6/discord_game_sdk.zip).
- Grab the DLL from `discord_game_sdk.zip` in the `lib` directory and put it in your project's `lib` directory.

## Documentation

If you need documentation, look at [**the official Game SDK docs**](https://discord.com/developers/docs/game-sdk/sdk-starter-guide); this was made following the official documentation.

## Features

- Should be working:
  - **ActivityManager**
  - **ImageManager**
  - **NetworkManager**
  - **RelationshipManager**
  - **StorageManager**
  - **UserManager**

- Should be working, but need more testing:
  - **AchievementManager** (not tested at all)
  - **ApplicationManager** (especially the functions `GetTicket` and `ValidateOrExit`)
  - **LobbyManager**
  - **OverlayManager**
  - **StoreManager** (not tested at all)
  - **VoiceManager**

## Contributing

The code needs **more comments, type hinting**. You can also implement the **missing features**, or add **more tests**. Feel free to open a **pull request**!

You can also **report issues**. Just open an issue and I will look into it!

### Todo List

- Better organisation of submodules.
- Docs.
- CI/CD,

## Examples

You can find more examples in the `examples/` directory.

### Create a Discord instance

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.Default)

# Don't forget to call RunCallbacks
while 1:
    time.sleep(1/10)
    app.RunCallbacks()
```

### Get current user

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.Default)

userManager = app.GetUserManager()


def onCurrUserUpdate():
    user = userManager.GetCurrentUser()
    print(f"Current user : {user.Username}#{user.Discriminator}")


userManager.OnCurrentUserUpdate = onCurrUserUpdate

# Don't forget to call RunCallbacks
while 1:
    time.sleep(1/10)
    app.RunCallbacks()

```

### Set activity

```python
import time

import discordsdk as dsdk

app = dsdk.Discord(APPLICATION_ID, dsdk.CreateFlags.Default)

activityManager = app.GetActivityManager()

activity = dsdk.Activity()
activity.State = "Testing Game SDK"
activity.Party.Id = "my_super_party_id"
activity.Party.Size.CurrentSize = 4
activity.Party.Size.MaxSize = 8
activity.Secrets.Join = "my_super_secret"


def callback(result):
    if result == dsdk.Result.Ok:
        print("Successfully set the activity!")
    else:
        raise Exception(result)


activityManager.UpdateActivity(activity, callback)

# Don't forget to call RunCallbacks
while 1:
    time.sleep(1/10)
    app.RunCallbacks()
```
