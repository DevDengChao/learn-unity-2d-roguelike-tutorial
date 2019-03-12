# 2D Roguelike tutorial

这篇文章是我通过 [2D Roguelike tutorial](https://unity3d.com/learn/tutorials/s/2d-roguelike-tutorial) 
项目在学习 Unity 的使用时留下的记录. 

官方教程中提供的视频来自 YouTube, 国内有搬运工已经把整套视频搬到 
[Bilibili](https://www.bilibili.com/video/av5480978) 了.

[TOC]

## Rules

通过试玩, 我发现了这个小游戏的一些基础的游戏规则:

0. 回合制游戏, 由玩家移动触发回合.
0. 玩家可以通过 `WASD` 和 `↑↓←→` 进行移动.
0. 地图中的可移动区域为8x8大小, 四周由外墙 (outerWall) 包围.
0. 可移动区域内会生成位置随机, 数量随机的障碍物 (wall), 水果 (food) 和饮料 (soda).
0. 可移动区域内会生成位置随机, 数量逐渐增多的敌人 (enemy).
0. 玩家 (player) 初始生命值 (foodPoint) 为 100, 碰到水果提升 10 点, 碰到饮料
提升 20 点, 被敌人攻击时, 根据敌人类型丢失 10 点或 20 点.
0. 障碍物有 3 点生命值, 被玩家攻击第一次以后会更换材质, 被摧毁后允许玩家和敌人通过.

## File tree

```
根目录
 ┣ Assets (资产目录, 存放源代码, 素材的地方, 应该被 VCS 管理)
 ┃ ┣ _Complete-Game (完整示例)
 ┃ ┃ ┣ Animation (动画示例)
 ┃ ┃ ┣ Prefabs (预制件示例)
 ┃ ┃ ┣ Scenes (场景示例)
 ┃ ┃ ┣ Scripts (脚本示例)
 ┃ ┃ ┣ _Complete-Game.unity (完整示例的主场景)
 ┃ ┃ ┗ (**.meta (各个文件/文件夹的元数据文件, 由 Unity3D 编译产生, 用来记录 Inspector 中的各种数据))
 ┃ ┣ Audio (音频素材)
 ┃ ┣ Fonts (字体素材)
 ┃ ┣ (Plugins (插件目录))
 ┃ ┣ Sprites (贴图目录)
 ┃ ┣ TutorialInfo (教程目录)
 ┃ ┗ Readme.asset (使用说明对应的资产)
 ┣ Library  (库目录, 存放 Unity3D 编译期产物的地方, 应该被 VCS 忽略)
 ┣ Logs (编译期日志文件夹, )
 ┣ obj (C# 脚本编译产物的地方)
 ┣ Packages (依赖目录? 应该被 VCS 管理)
 ┃ ┗ manifest.json (清单文件, 记录了这个项目的第三方依赖等)
 ┣ ProjectSettings (工程配置, 应该被 VCS 管理)
 ┃ ┣ *.asset (各种资源管理器对应的元数据)
 ┃ ┗ ProjectVersion.txt (目前只记录了 Unity3D 编辑器的版本号)
 ┣ Temp (用途不明的临时文件夹, 应该被 VCS 忽略)
 ┣ *.csproj (Visual studio 编译项目时生产的文件, 应该被 VCS 忽略)
 ┗ *.sln (Visual studio 的项目管理文件, 应该被 VCS 管理)
```

## _Complete-Game

该目录下包含整个教程完整的示例, 是我学习这个项目的主战场.

