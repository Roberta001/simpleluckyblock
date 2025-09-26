# SimpleLuckyBlock (English)

A powerful, highly customizable Lucky Block plugin designed for modern Minecraft servers (Paper/Spigot).

This plugin is built around a core mechanic of **"Luck Value" ranging from -100 to +100**. It utilizes a weighted probability model to deliver an experience filled with suspense and surprise.

## ✨ Features

- **Luck Value System**: Every lucky block has a luck value from -100 (extremely unlucky) to +100 (extremely lucky), which directly influences the events that occur when it's broken.
- **Anvil Modifiers**: A unique gameplay mechanic! Players can use an anvil to combine a lucky block with specific items (definable in `config.yml`) to increase or decrease its luck value.
- **Weighted Probability Model**: Say goodbye to pure RNG! The closer a block's luck value is to an event's defined luck, the **exponentially higher** the chance of that event triggering. This makes modifying the luck value truly meaningful.
- **Powerful Action System**: Configure a vast array of outcomes in `events.yml` to create unique events:
  - `[ITEM] <item> <amount> [nbt]`: Drops a specified item with amount and NBT.
  - `[ENTITY] <entity> <amount> [nbt]`: Spawns an entity at the block's location, with NBT support.
  - `[STRUCTURE] <filename> [x y z]`: Pastes a structure schematic. Requires **WorldEdit**.
  - `[LOOT] <loot_table_key>`: Generates loot from a vanilla loot table.
  - `[COMMAND] <command>`: Executes a command from the console.
  - `[PLAYER_COMMAND] <command>`: Makes the player execute a command.
  - `[MESSAGE] <message>`: Sends a message to the player (supports `&` color codes).
- **Variables & Placeholders**:
  - Full support for **PlaceholderAPI** placeholders.
  - Built-in `{rand:min-max}` variable to generate random numbers within actions.
- **Excellent Compatibility**:
  - Intelligently detects and respects most region protection plugins (e.g., WorldGuard), preventing the `[STRUCTURE]` action from griefing protected areas.
- **Visual Polish**: Placed lucky blocks display a dynamic, rotating item, providing better visual feedback.

## ⚙️ Dependencies

- **WorldEdit**: **Required** if you plan to use the `[STRUCTURE]` action.
- **PlaceholderAPI**: Optional. If installed, you can use PAPI placeholders in your event actions.

## 🛠️ Installation

1. Download the latest `.jar` file from the releases page.
2. Place it in your server's `plugins` folder.
3. Restart the server. The plugin will generate `config.yml`, `events.yml`, and a `structures` folder.

## 📜 Commands & Permissions

| Command | Description | Permission |
| --- | --- | --- |
| `/luckyblock give <player> [luck]` | Gives a player a lucky block with a specific luck value. | `simpleluckyblock.command.give` |
| `/luckyblock reload` | Reloads the plugin's configuration and events. | `simpleluckyblock.command.reload` |
| `/luckyblock trigger <eventID> [player]` | Triggers a specific event for a player (for debugging). | `simpleluckyblock.command.trigger` |

By default, OPs have all permissions.

## 🔧 Configuration

### `config.yml`

```yaml
# Items that can modify a lucky block's luck value in an anvil.
# Format: 'MATERIAL_NAME': luck_change
luck-modifiers:
  DIAMOND: 5
  EMERALD: 8
  NETHER_STAR: 100
  ROTTEN_FLESH: -2
  POISONOUS_POTATO: -5

# Whether to check for region permissions when pasting structures.
# It is highly recommended to keep this true to prevent griefing.
structures:
  check-permissions: true
```

### `events.yml`

This file defines all possible events that can occur when a lucky block is broken.

```yaml
events:
  # A unique ID for the event
  good_diamond_rain:
    # The base luck for this event. The closer the block's luck is to this value, the higher the chance.
    luck: 80
    # A list of actions to execute when this event is triggered.
    actions:
      - "[MESSAGE] &aThe gods of fortune smile upon you!"
      - "[ITEM] minecraft:diamond 10" # Drops 10 diamonds
      - "[ENTITY] minecraft:cow 5 {CustomName:'\"Lucky Cow\"'}" # Spawns 5 lucky cows
  
  ultimate_gear:
    luck: 100
    actions:
      - "[MESSAGE] &6&lYou have found the ultimate treasure!"
      - "[ITEM] minecraft:netherite_sword 1 {Enchantments:[{id:sharpness,lvl:10}]}"
      - "[LOOT] minecraft:chests/end_city_treasure" # Pulls loot from the end city treasure loot table
  
  bad_tnt_trap:
    luck: -75
    actions:
      - "[MESSAGE] &cOh no! It's a trap!"
      - "[COMMAND] summon tnt ~ ~1 ~" # Summons a TNT above the player
  
  random_structure:
    luck: 50
    actions:
      - "[MESSAGE] &eA mysterious structure has appeared!"
      # Pastes the schematic from structures/small_house.schem at the block's location
      # with an offset of x=0, y=0, z=0
      - "[STRUCTURE] small_house.schem 0 0 0" 

  random_number_example:
    luck: 10
    actions:
      # {rand:5-20} will be replaced by a random integer between 5 and 20.
      - "[ITEM] minecraft:iron_ingot {rand:5-20}"
      - "[PLAYER_COMMAND] say I got {rand:1-100} luck points!"
```

## ❓ How It Works

1.  **Obtain**: An admin gives a lucky block to a player using the `/luckyblock give` command.
2.  **Modify (Optional)**: The player can place the lucky block in an anvil with a modifier item to change its luck value.
3.  **Place & Break**: The player places the lucky block and breaks it without a Silk Touch tool.
4.  **Trigger**: The plugin uses its weighted algorithm to select a random event from `events.yml` based on the block's luck value.
5.  **Execute**: All actions associated with the chosen event are executed.
---
# SimpleLuckyBlock - 简单幸运方块 (中文简体)

一款为 Minecraft 服务端 (Paper/Spigot) 设计的、高度可定制的、功能强大的幸运方块插件。

它围绕着 **-100 到 +100 的“幸运值”** 核心机制，通过科学的加权概率模型，为玩家带来充满惊喜和期待的体验。

## ✨ 特点

- **幸运值系统**: 每个幸运方块都有一个范围在 -100 (极度不幸) 到 +100 (极度幸运) 之间的幸运值，它直接影响着方块被破坏时可能发生的事件。
- **铁砧强化**: 独创的玩法！玩家可以在铁砧中，通过消耗在 `config.yml` 中定义的物品，来提升或降低幸运方块的幸运值。
- **加权概率模型**: 告别纯粹的随机！方块的幸运值越接近事件所定义的幸运值，触发该事件的概率就呈 **指数级增长**。这使得幸运值的增减变得极具意义。
- **强大的事件动作**: 在 `events.yml` 中，你可以配置丰富多样的动作来创造独一无二的事件：
  - `[ITEM] <item> <amount> [nbt]`: 掉落指定数量和NBT的物品。
  - `[ENTITY] <entity> <amount> [nbt]`: 在方块位置生成实体，支持NBT。
  - `[STRUCTURE] <filename> [x y z]`: 粘贴建筑。需要 **WorldEdit** 支持。
  - `[LOOT] <loot_table_key>`: 从原版战利品表中抽取物品。
  - `[COMMAND] <command>`: 以控制台身份执行指令。
  - `[PLAYER_COMMAND] <command>`: 让玩家执行指令。
  - `[MESSAGE] <message>`: 向玩家发送一条消息（支持颜色代码 `&`）。
- **变量与占位符**:
  - 支持 **PlaceholderAPI** 变量。
  - 内置 `{rand:min-max}` 变量，用于在动作中生成随机数。
- **优秀的兼容性**:
  - 能够智能检测并兼容大多数领地插件 (如 WorldGuard)，防止 `[STRUCTURE]` 动作破坏受保护的区域。
- **精美的视觉效果**: 幸运方块在放置后会显示一个动态旋转的物品，提供了更好的视觉反馈。

## ⚙️ 前置插件

- **WorldEdit**: 如果你需要使用 `[STRUCTURE]` 动作来粘贴建筑，此为 **必需** 前置。
- **PlaceholderAPI**: 可选前置。如果安装，你可以在事件动作中使用PAPI变量。

## 🛠️ 安装

1. 下载最新的 `.jar` 文件。
2. 将其放入你服务器的 `plugins` 文件夹中。
3. 重启服务器。插件会自动生成 `config.yml` 和 `events.yml` 配置文件，以及一个 `structures` 文件夹。

## 📜 指令与权限

| 指令 | 描述 | 权限 |
| --- | --- | --- |
| `/luckyblock give <玩家> [幸运值]` | 给予玩家一个指定幸运值的幸运方块。 | `simpleluckyblock.command.give` |
| `/luckyblock reload` | 重载插件的配置文件和事件。 | `simpleluckyblock.command.reload` |
| `/luckyblock trigger <事件ID> [玩家]` | 为玩家触发一个指定的事件（用于调试）。 | `simpleluckyblock.command.trigger` |

默认OP拥有所有权限。

## 🔧 配置

### `config.yml`

```yaml
# 在铁砧中可以用来改变幸运方块幸运值的物品
# 格式: '材料名': 幸运值改变量
luck-modifiers:
  DIAMOND: 5
  EMERALD: 8
  NETHER_STAR: 100
  ROTTEN_FLESH: -2
  POISONOUS_POTATO: -5

# 粘贴建筑时是否检查领地权限
# 强烈建议保持为 true，以防止破坏玩家领地
structures:
  check-permissions: true
```

### `events.yml`

这里定义了当幸运方块被破坏时可能发生的所有事件。

```yaml
events:
  # 事件的唯一ID
  good_diamond_rain:
    # 此事件的幸运值基准。方块的幸运值越接近这个值，触发概率越高。
    luck: 80
    # 事件触发时执行的动作列表
    actions:
      - "[MESSAGE] &a幸运之神眷顾了你！"
      - "[ITEM] minecraft:diamond 10" # 掉落10个钻石
      - "[ENTITY] minecraft:cow 5 {CustomName:'\"幸运牛\"'}" # 生成5只幸运牛
  
  ultimate_gear:
    luck: 100
    actions:
      - "[MESSAGE] &6&l你获得了终极宝藏！"
      - "[ITEM] minecraft:netherite_sword 1 {Enchantments:[{id:sharpness,lvl:10}]}"
      - "[LOOT] minecraft:chests/end_city_treasure" # 从末地城宝箱战利品表中抽奖
  
  bad_tnt_trap:
    luck: -75
    actions:
      - "[MESSAGE] &c噢不！这是一个陷阱！"
      - "[COMMAND] summon tnt ~ ~1 ~" # 在玩家头顶生成一个TNT
  
  random_structure:
    luck: 50
    actions:
      - "[MESSAGE] &e一个神秘的建筑出现了！"
      # 将 structures/small_house.schem 粘贴在方块位置
      # 偏移量为 x=0, y=0, z=0
      - "[STRUCTURE] small_house.schem 0 0 0" 

  random_number_example:
    luck: 10
    actions:
      # {rand:5-20} 会被替换为一个5到20之间的随机整数
      - "[ITEM] minecraft:iron_ingot {rand:5-20}"
      - "[PLAYER_COMMAND] say 我得到了 {rand:1-100} 点幸运积分！"
```

## ❓ 工作原理

1.  **获取**: 管理员通过 `/luckyblock give` 指令给予玩家幸运方块。
2.  **强化 (可选)**: 玩家可以将幸运方块与特定材料放入铁砧，来改变其幸运值。
3.  **放置与破坏**: 玩家放置幸运方块，然后用手或工具将其破坏（非精准采集）。
4.  **触发**: 插件会根据方块的幸运值，使用加权算法从 `events.yml` 中随机选择一个事件。
5.  **执行**: 执行被选中事件的所有动作。
