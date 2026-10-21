# AI Pet Companion (Fabric, Minecraft 26.3)

A tamed pet (cat, kitten, wolf, puppy or parrot) that follows you, **sees your world** (as text),
**remembers what you did together** (saved per world), and **chats with you** using a cloud AI.
It's server-side only: no client mod is needed. It works in single-player (the built-in server) and on servers.

> Status: written for Minecraft 26.3 (Fabric Loader 0.19.5, Fabric API 0.161.0+26.3, Java 25).
> It has NOT been compiled or play-tested yet. If the build shows errors, send them to Claude to fix.

## 1. Build the mod (pick one)

### A) Free online build with GitHub (no installs)
1. Create a free account at github.com, make a new repository, upload ALL files from this folder (including the hidden `.github` folder).
2. Open the **Actions** tab, wait for the "build" run to finish (green check), open it, download the **aipet-mod** artifact.
3. Unzip it; inside is `aipet-1.0.0.jar`.
If the run is red, open it and copy the error text.

### B) On your PC
Install Java 25 (Temurin) and Gradle 9.6, then run `gradle build` in this folder. The jar appears in `build/libs/aipet-1.0.0.jar`.

## 2. Install in Prism Launcher
1. Create a new instance: Minecraft **26.3**, mod loader **Fabric**. (Prism should pick Java 25 automatically; if not, set it in Settings > Java.)
2. Instance > Edit > **Mods** > Download mods > search "Fabric API", pick the version for 26.3.
3. In the same Mods tab click **Add file** (or open the mods folder) and add `aipet-1.0.0.jar`.
4. Launch once, then close. This creates `config/aipet.json` (Instance > Folder > Minecraft folder > config).

## 3. Add your AI key
Open `config/aipet.json` and set `api_key`.

Google Gemini (free tier, default):
    "provider": "openai",
    "base_url": "https://generativelanguage.googleapis.com/v1beta/openai",
    "model": "gemini-2.5-flash"

OpenAI:
    "provider": "openai", "base_url": "https://api.openai.com/v1", "model": "gpt-4o-mini"

Claude:
    "provider": "anthropic", "model": "claude-haiku-5-5"

In-game, run `/pet reload` after editing.

## 4. Play
- `/pet summon` - call your pet. `/pet skins`, then `/pet skin <name>` to change its look (Tab autocompletes).
- Talk to it by putting its name in chat ("Pip, what do you see?"), or start a message with "pet". Or use `/pet say <message>`.
- `/pet name <name>`, `/pet personality <text>`, `/pet memory`, `/pet forget`, `/pet dismiss`, `/pet help`.
- It comments by itself when you die, get low on health, mine diamonds, or a creeper gets close (`proactive_comments` in the config; `comment_cooldown_seconds` limits how often).

## How it works
- **Sees the world:** every message includes a text snapshot: your health, hunger, held item, biome, dimension, nearby mobs and notable blocks, and what you've been doing recently.
- **Memory:** your actions (blocks mined/placed, kills, deaths, travel) are logged and summarised into short notes every few minutes. Notes are saved in `<world>/aipet/<your-id>.json` and given to the AI each time you talk. Without an API key, simple raw notes are stored.
- **Pet body:** a vanilla tamed animal (invulnerable, never despawns). Wolves/puppies may still fight mobs, since that is vanilla wolf behaviour. Cats may scare creepers.

## Known limits
- Skins are vanilla animals (cat/kitten/wolf/puppy/parrot). Coat colour is random when summoned. Summon again to re-roll.
- Time of day is estimated from sky brightness, and only works when you are under open sky.
- The AI costs API credit (or uses your free quota) per message.
