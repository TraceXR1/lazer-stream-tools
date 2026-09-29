# osu!lazer streaming tools

Welcome, here you will find a toolkit for **streaming osu! tournaments hosted on the lazer client**. Please jump [here](#installation) if you want to install it ASAP

As you might know, there are **no official tools** for streaming tournaments on lazer client, unlike *stable*, which has special stream client for multi-spectating and tournament overlay. So I decided: *"why not try make something for streaming lazer tourneys?"*

Of course, there are actually modded forks of tournament overlay with room spectating ability embedded into it. But here is another problem: you can't connect to multiplayer room in lazer when using **unofficial build** of the client unless you have **official support**, as I was told by the people I talked to about hosting lazer tourneys

And so, I had to find a way to stream lazer tournament **without any official support**

## So how did I do it?

I decided to use **tosu memory reader** (modified, [see here](#tosu-modifications)) and **custom bridge** to receive data from tosu and write it in real time into the stable's **file-based IPC format**. The rough idea is:

<div align=center>osu!lazer → tosu (fork) → WebSocket → IPC Emulator → ipc/*.txt → tournament client → OBS</div>

### File-based IPC format

`ipc.txt`
1. Current beatmap ID
2. Current required mods bitnumber

`ipc-scores.txt`
1. Left side's current score
2. Right side's current score

`ipc-channel.txt`
1. Room's chat channel ID

`ipc-state.txt`
1. Current state (Idle/Playing/Ranking)

For convenience purposes tournament overlay was modified too, [see here](#tournament-overlay-modifications)

## Versions compatibility

Memory reading is **very version-sensitive**, but this should not be a worry as tosu will *automatically* download new offsets when the new version of osu!lazer will be released. In case of a **breaking change** and me not noticing it, please give me a heads up on Discord: `@tracexr` or create an [issue](https://github.com/TraceXR1/lazer-stream-tools/issues)

## Installation

- Download [latest release](https://github.com/TraceXR1/lazer-stream-tools/releases/latest) for your system
- Extract the archive
- Launch #finish-later

### Authorization

- Go to the [osu! website](https://osu.ppy.sh/home/account/edit#oauth)
- Scroll to the bottom of `OAuth` section and find the button `New OAuth Application`
- Create the new OAuth Application and set `Application Callback URL` to **exactly** `http://127.0.0.1:48732/`

> [!WARNING]
> Application Callback URL should be **exactly** `http://127.0.0.1:48732/`

- Copy `Client ID` and `Client Secret` and paste them into the tournament overlay
- Click on `Login`. You'll be taken to the browser page where you'll be asked to give authorization
- In case of success, you'll see the notice about **successful authorization** on the browser page and in the tournament overlay

> [!CAUTION]
> **Never** share your API credentials anywhere.
> If compromised — delete the application on the [osu! website](https://osu.ppy.sh/home/account/edit#oauth)

### Technical limitations

- This toolkit will only work on **Windows or Linux with x64 architecture**
- Ports `24050` and `48732` should be **free**

## FAQ

### Where can I report bugs or share my thoughts?

Please report the bugs by creating an [issue](https://github.com/TraceXR1/lazer-stream-tools/issues) on GitHub or DMing `@tracexr` on Discord

### How can I build binaries?

Clone this repository using this command:
```bash
git clone --recurse-submodules https://github.com/TraceXR1/lazer-stream-tools.git
```
If you've already cloned repository **without the flag**, please update submodules using:
```bash
git submodule update --init --recursive
```
Then please reference `README.md` files of each submodule
 
## tosu modifications

1. added ability to read lazer's fields, namely **scores in multiplayer room**, **chat id of multiplayer room** and **current required mods in multiplayer room** by @TraceXR1 on 2026-09-26
2. added state `spectating` by @TraceXR1 on 2026-09-26

All diffs can be found in [this repository](https://github.com/TraceXR1/tosu)

## Tournament overlay modifications

Tournament client was modified in numerous ways, but mainly these are **customization options**. One of the technical modifications is replacement of **password grant** login method with APIv2's **authorization code grant** due to inability to be logged in using login/password in *two clients at the same time*

## Licensing

**Disclaimer:** unofficial community project, **not affiliated with or endorsed by** ppy Pty Ltd or the tosu authors; "osu!" is a trademark of ppy Pty Ltd

This repository only aggregates the components below; each of them is distributed under **its own license**

**tosu:** Copyright (C) 2023-2026 Mikhail Babynichev <https://kotrik.ru/>; [LGPL-3.0](LICENSES/tosuapp/tosu/LICENSE) license; [fork repository](https://github.com/TraceXR1/tosu)

**osu!lazer:** Copyright (c) 2025 ppy Pty Ltd <contact@ppy.sh>; [MIT](LICENSES/ppy/osu/LICENCE) license; [fork repository](https://github.com/TraceXR1/osu-tclient)

**IPC Emulator:** Copyright (c) 2026 TraceXR; [MIT](LICENSES/TraceXR1/tosu-ipc-emulator/LICENSE) license; [repository](https://github.com/TraceXR1/tosu-ipc-emulator)