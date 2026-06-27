# VanillaDatapackFramework [VDF]

Welcome to the VanillaDatapackFramework [VDF], an elite, production-ready template designed for Minecraft 1.21 and beyond. This framework completely abandons legacy NBT formats in favor of modern Data Components and emphasizes maximum server performance (TPS).

## Version Compatibility & Pack Formats

You do not need to rewrite your code when Minecraft updates. To upgrade the data pack for newer versions, simply change the `pack_format` value inside your `pack.mcmeta` file.

**Format Reference Guide:**
- `48` = Minecraft 1.21 - 1.21.1
- `57` = Minecraft 1.21.2 - 1.21.3
- `61` = Minecraft 1.21.4
- `71` = Minecraft 1.21.5
- `80` = Minecraft 1.21.6
- `81` = Minecraft 1.21.7 - 1.21.8
- `88` = Minecraft 1.21.9 - 1.21.10
- `94` = Minecraft 1.21.11
- `101` = Minecraft 26.1 - 26.1.2
- `107` = Minecraft 26.2

*(For more details and future versions beyond 26.2, please consult the official [Minecraft Wiki Data Pack Format](https://minecraft.wiki/w/Pack_format#Data_pack_format_history)).*

## Framework Architecture

Understanding the workspace is crucial for maintaining clean and optimized code:

* **`data/minecraft/tags/function/`**
  Contains `load.json` and `tick.json`. These interface directly with the Minecraft engine to execute your root functions upon initialization and every game tick (20 times per second).
* **`data/vdf/function/`**
  The core logic directory for your scripts (`.mcfunction` files). Keep these modular and organized.
* **`data/vdf/predicate/`**
  Highly optimized JSON files for complex conditional checks (e.g., verifying item components, entity states). Always use these over raw NBT checks in your execute commands.
* **`data/vdf/advancement/`**
  Event-driven triggers. Use these to passively detect player actions (like consuming an item, attacking, or taking damage) instantly without straining the server's tick loop.

## Elite Development Principles

### 1. Data Components Over NBT
Legacy item NBT (e.g., `{CustomModelData:1}`) is completely obsolete. You must strictly use Data Components.
- **Creation:** `give @s minecraft:diamond_sword[minecraft:custom_model_data=7001]`
- **Modification:** `/item modify entity @s weapon.mainhand ...`

### 2. Server Optimization (TPS) First
Never place heavy, constant checks inside `tick.mcfunction`.
- **Bad Practice:** Running `/execute as @a[nbt={...}] run ...` 20 times a second.
- **Elite Practice:** Use an advancement trigger to run a target function *only* in the exact moment the required event occurs.

### 3. Modern Data Storage
Do not summon invisible `armor_stand` or `marker` entities to hold variables unless a specific 3D world coordinate is absolutely required. Always use `storage` for global variables, states, and mathematical operations:
- `data modify storage vdf:main core.my_variable set value 1`

### 4. Resource Pack Integration
This framework is built with visual modifications in mind. When designing custom items, map your `minecraft:custom_model_data` component IDs directly to the model overrides in your Resource Pack. Use standardized ID ranges (e.g., 7000-7999 for custom weapons) to prevent conflicts.