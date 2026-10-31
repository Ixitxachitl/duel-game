# Duel

A water-pistol duel for chat, as an OBS browser source. A chatter types `!duel somebody` and a little pixel-art
Old West town grows in along the bottom of the screen against a sunset: a saloon with horses tied up out front,
a jail, a bank, a hotel and a general store, with cacti and tumbleweeds on a strip of see-through ground. The
two of them walk out of the saloon's swinging doors into the middle of the street, stand back to back, take
five paces, turn and... draw. The winner soaks the loser, whose own pistol only dribbles, and knocks their hat
off. Nobody gets hurt. Then it all shrinks away, the sun sets, and there's nothing on screen until the next duel.

Everything is a full 3D voxel model, drawn at a low resolution and seen flat on with no perspective, so it looks
like 2D pixel art; only the ground shows a little depth (the 2.5D look). You see the models are solid when they
turn, or twist as they grow in and out. The duelists are little chibi cowboys and cowgirls: their shirt is their
chat color, and the same chatter always gets the same hat, hair and pistol.

Twitch chatters who have set their pronouns with the [Chat Pronouns](https://pronouns.alejo.io/) extension get
a cowboy for he/him and a cowgirl for she/her; everyone else gets either. (Turn that off with `pronouns=0`.)

Lives at <https://duel.ixitxachitl.com/>.

## Setup

In OBS: **Add → Browser**, URL `https://duel.ixitxachitl.com/?channel=yourchannel`, at your canvas size (e.g.
1920×1080). The background is transparent inside OBS.

It pops up in a small box along the bottom of the screen, in the middle, and everything stays inside it. Set
**Where it shows** (`layout=wide`) to use the whole screen instead, with the sky and lights going right up to the
top; **Box width** (`boxsize`, % of the screen, default 40) sets how big the box is.

Right-click the source → **Interact** and move the mouse over it:

- The bar in the bottom-right corner starts a duel between two names you type, and **Stop** ends one early.
- The **gear** opens the settings: your Twitch / Kick channel, TikTok or YouTube, the command, the cooldown,
  who can start a duel, pronoun lookups, the pixel size and the sound volume.

Every setting can also be a URL parameter, which overrides it. The full list is in the comment at the top of
`index.html`. `?demo=1` starts a duel every few seconds, for trying it out.

## Chat

| Command        | What                                                                                  |
| -------------- | ------------------------------------------------------------------------------------- |
| `!duel name`   | duels `name` (`@name` works too), if they've chatted in the last 10 minutes           |
| `!duel`        | duels someone picked at random from the people who chatted in the last 10 minutes      |

- After a duel there's a **cooldown** (60 seconds by default) before the next one. The broadcaster skips it,
  and so can mods if you turn that on.
- **Who can start one** is everyone by default, or mods and VIPs, mods, or only the broadcaster. **These
  people can too** adds particular names.
- You can't duel yourself, and while a duel is on, other `!duel`s are ignored.

Chat is read anonymously: Twitch and Kick directly, YouTube with a Data API key (or a WebSocket bridge), TikTok
through a bridge such as TikFinity.

Custom integrations (Streamer.bot, etc.) can call `window.duel.start(challenger, opponent, { winner })` (winner 1
or 2, otherwise a coin flip) and `window.duel.stop()`. These skip the cooldown and the 10-minute rule.

## Screenshots

Headless Chrome doesn't run animation frames under `--virtual-time-budget`, so add `timer=1` (frames on timers
instead):

```sh
chrome --headless=new --hide-scrollbars --window-size=1920,1080 --use-angle=swiftshader --enable-unsafe-swiftshader \
  --virtual-time-budget=13000 --screenshot=shot.png "https://duel.ixitxachitl.com/?demo=1&volume=0&timer=1"
```
