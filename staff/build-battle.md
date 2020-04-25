# 建筑比赛·技术流程

**技术流程**是「保证建筑比赛按照**活动流程**有序进行的一道保障」。

## Why 技术流程

> 试想一下，4月25日发布了建筑比赛的预告，说比赛会在5月1日在创造服开始。没有技术上的绝对限制，玩家可能会在5月1日**之前**就开始在比赛场地建造作品。同样，假设比赛规定5月3日24点结束，玩家可能会**超时**并继续建造。

以上情况都不利于比赛的公平。

## 技术流程的范畴

技术流程不讲：比赛主题、比赛时间、比赛规则。这些属于**活动流程**的范畴。

**技术流程服务于活动流程。**也就是说，如果活动流程有需要强制措施才能实现的规则（比如玩家只能在规定时间之后开始建造），请直接联系小米说明活动流程的需求。小米会考虑什么样的技术流程才可以保证活动流程按所期望的进行。

## 具体的技术流程

下面进入正文：具体如何操作。

### 后台的操作

后台需要的所有操作都在[后台备忘录](#console)。游戏内的主持人可以忽略这一步。

### 主持人的操作

1. （比赛开始的时间到了！）
2. 主持人进入[自由创造服](/mc-servers/creative.md)
3. 主持人打开`快捷菜单`-`建筑比赛菜单`-`主持人菜单`
4. 主持人**开放**比赛世界的`建造权限`和`圈地权限`
5. （比赛开始！）
6. （若干天过去后...）
7. （比赛结束的时间到了！）
8. 主持人打开`主持人菜单`
9. 主持人**关闭**比赛世界的`建造权限`和`圈地权限`
10. 技术流程到此结束，接下来就是活动流程

## 后台备忘录 :id=console

?> 以下内容仅仅供有后台权限的人参考。建筑比赛的游戏内主持人可以忽略以下内容。

### 控制建造

`控制建造`已整合进`主持人菜单`，这部分内容由游戏内的主持人控制。

- `/rg flag __global__ build -w build_{第几届} deny` 关闭整个世界的建造权限
- `/rg flag __global__ build -w build_{第几届}` 启用整个世界的建造权限

### 调整菜单

#### menu.yml

更新快捷菜单`{创造服目录}/plugins/BossShopPro/shops/menu.yml`，把下面图标里的`name`更新一下：

- 如果比赛已结束，需要标明`&c(已结束)`（注意颜色代码）
- 如果比赛即将开始/进行当中，需要标明`&a(即将开始)`或`&a(进行当中)`

具体的配置文件如下：

```yaml
  game:
    MenuItem:
      - type:bookshelf
      - amount:1
      - name:&a[v] &7参加 &e&l第{x}届个人建筑比赛 {&c(已结束)|&a(即将开始)|&a(进行中)} # 更新下 "name:" 后面的内容
    RewardType: shop
    Reward: game
    PriceType: nothing
    InventoryLocation: 12
```

#### game.yml

更新快捷菜单`{创造服目录}/plugins/BossShopPro/shops/game.yml`，更新下面图标的`Reward`：

```yaml
  go-battle:
    MenuItem:
    - type:ender_pearl
    - amount:1
    - name:&a[v] &7前往 &6&l建筑比赛世界
    - lore:&7
    - lore:&7前往当前建筑比赛所在的世界.
    - lore:&7
    - lore:&7这是参与建筑比赛的第一步～
    RewardType: playercommand
    Reward:
    - mv tp {当前比赛世界} # 这里更新成新一届建筑比赛所在的世界
    PriceType: nothing
    InventoryLocation: 11
    CloseShopAfterPurchase: true
```

#### game_host.yml

更新快捷菜单`{创造服目录}/plugins/BossShopPro/shops/game_host.yml`，更新下面图标的`Reward`：

```yaml
  禁止圈地权限:
    MenuItem:
    - type:player_head
    - customskull:eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvNjNiYTAyNGMwNzRiNWQ5YWRmNTJlY2VjZGVkNmI0YTlkMTRmMWEyZjQxMzg2MzVkYjc5YTk1NmZiYWIxNDM1MCJ9fX0=
    - amount:1
    - name:&c[swords] &f&l关闭 &6&l圈地权限
    RewardType: command
    Reward:
    - lp group default_build parent remove game_ongoing world={当前比赛世界} # 更新 world= 后面的世界为当前的建筑比赛世界
    PriceType: nothing
    InventoryLocation: 20
    CloseShopAfterPurchase: true
    Message: '已关闭玩家的圈地权限.'
  开放圈地权限:
    MenuItem:
    - type:player_head
    - customskull:eyJ0ZXh0dXJlcyI6eyJTS0lOIjp7InVybCI6Imh0dHA6Ly90ZXh0dXJlcy5taW5lY3JhZnQubmV0L3RleHR1cmUvMWI2N2VkYjZhYjIxMjlhM2I0MzlkZGU5ODg2OWUwNWFmODFjODZkOTQ4MTVlMzdkMzEzNTliY2QzZGRmNDU4ZCJ9fX0=
    - amount:1
    - name:&a[v] &f&l开放 &6&l圈地权限
    RewardType: command
    Reward:
    - lp group default_build parent add game_ongoing world={当前比赛世界} # 更新 world= 后面的世界为当前的建筑比赛世界
    PriceType: nothing
    InventoryLocation: 21
    CloseShopAfterPurchase: true
    Message: '已开放玩家的圈地权限.'
```

### 调整权限

#### group: game_basic

不管有没有进行中的比赛，这是玩家始终继承的权限组。

该权限组的作用是让玩家可以传送地皮等等，以便在比赛结束后回来参观。

无论如何，这个权限组**不应该**包含`领新地皮`和`清空地皮`的权限。

```group: game_basic
plots.continue
plots.done
plots.home
plots.info
plots.set.biome
plots.set.home
plots.use
plots.visit
plots.visit.other
plots.list
plots.list.*
```

#### group: game_ongoing

这是玩家仅仅在比赛期间才继承的权限组。

该权限组的作用是控制玩家是否能`领新地皮`和`清空地皮`。

```group: game_ongoing
plots.claim
plots.delete
plots.plot.1
plots.plot.2
plots.plot.{x} # 这个是玩家可以声明的最大地皮数。随着往届建筑比赛越来越多，这个数值也应该相应提高
```

#### 比赛开始之前的操作

<!-- 比赛期间应该让`default_build`组继承`game_ongoing`组：

```
lp group default_build parent add game_ongoing world={当前比赛的世界名}
``` -->

设置可声明的地皮数量上限 +1：

```
lp group game_ongoing permission set plots.plot.{原最大数量+1}
```

<!-- #### 比赛结束之后的操作

比赛结束后，需要移除`game_ongoing`的继承：

```
lp group default_build parent remove game_ongoing world={当前比赛的世界名}
``` -->

### 设置卫星地图

要**正确**启用新一届建筑比赛的卫星地图，需要在`{创造服文件夹}/plugins/dynmap/worlds.txt`中加上新的地图配置。

建议直接复制上一届建筑比赛的地图配置，然后分别修改`key:name`和`key:title`这两个的`value`。其中，`key:name`指世界的技术名。`key:title`指在[卫星地图](http://map.mimaru.me:8123/creative)网页上看到的世界的标题。

以下是第七届和第八届的卫星地图配置。

> [!note|label:关于 title 的设置]
> 考虑到卫星地图一般会在比赛正式之前就设置好，而`title`中又包含了建筑比赛的主题，所以在当前比赛正式开始之前，主题应该隐藏起来（在下面的例子里，主题用`???`代替了）以避免过早泄题。等到比赛正式开始后，再把`???`改成当前实际的比赛主题，然后执行`/dynmap reload`重启卫星地图。

> [!note|label:地图配置在文件中的顺序]
> 这些地图配置在文件`worlds.txt`中是有顺序的，这些顺序会直接体现在卫星地图的网页上。这点你可以仔细观察`worlds.txt`中地图配置和实际网页呈现之间的关系。例如下面的例子中，`build_8`在`build_7`之上。

```yaml
  - name: build_8
    title: "建筑比赛第八届 : ???"
    enabled: true
    maps:
      - class: org.dynmap.hdmap.HDMap
        name: surface
        title: "立体视图"
        prefix: t
        perspective: iso_SE_30_vlowres
        shader: stdtexture
        lighting: shadows
  - name: build_7
    title: "建筑比赛第七届 : 名胜古迹"
    enabled: true
    maps:
      - class: org.dynmap.hdmap.HDMap
        name: surface
        title: "立体视图"
        prefix: t
        perspective: iso_SE_30_vlowres
        shader: stdtexture
        lighting: shadows
```

### 创建比赛世界

!> 每一届**新的**建筑比赛应该在**新的**超平坦地皮世界中进行，因为没有好办法区分地皮上的作品属于哪一届比赛。

创建比赛世界的指令是`/plot setup`。

输入该指令后，插件会问你问题，让你用指令`/plot setup <值>`设置好对应的参数。

每当你用`/plot setup <值>`设置好一个参数后，就会问你下一个问题，直到全部设置完毕。

请以下面的参数设置好：

- 第一个问题`What generator do you want?`，输入`/plot setup plotsquared`
- 第二个问题`What world type do you want?`，输入`/plot setup default`

从第三个问题开始，就是设置地皮的具体参数，例如地皮大小，地皮地板的方块种类。

这些参数请直接参考下面列出的地皮世界（`build_8`）的配置文件，这里就不再说了。

我已经给`/plot setup`会用到的参数加上了注释（`#`右边的内容）。

```yaml
  build_8: # 世界的名字，在你使用 /plot setup 进行到最后一步时会用到
    plot:
      height: 60 # 地皮高度，请保持全部一致
      size: 128 # 地皮尺寸为 128*128
      filling: stone # 地皮的地板之下填充为石头
      floor: grass_block # 地皮的地板设置为 grass_block
      bedrock: true # 地皮的最底层是否带基岩
      biome: FOREST # 默认生物群系是 FOREST
      auto_merge: false
      create_signs: true
    wall:
      block: stone_slab # 地皮边框为 stone_slab（地皮还未被 claimed 时）
      block_claimed: quartz_slab # 已经被 claimed 的地皮边框应该为 quartz_slab（石英台阶）
      filling: oak_planks # 边框之下填充为 oak_planks
      height: 60 # 围墙高度，请保持全部一致
    road:
      width: 7 # 路面宽度，别太宽，也别太窄，7 刚刚好
      height: 60 # 地面高度，请保持全部一致
      block: oak_planks # 路面之下填充为 oak_planks
    misc_spawn_unowned: false
    home:
      nonmembers: side
      default: side
    schematic:
      specify_on_claim: false
      on_claim: false
      file: 'null'
    economy:
      prices:
        merge: 100
        sell: 100
        claim: 100
      use: false
    chat:
      enabled: false
    limits:
      max-members: 128
    world:
      max_height: 256
      gamemode: creative
      min_height: 1
      border: false
    event:
      spawn:
        egg: false
        breeding: false
        custom: true
    natural_mob_spawning: false
    mob_spawner_spawning: false
    flags: {}
```