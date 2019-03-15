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

## Version control

第一次接触 Unity 工程, 在做版本控制的时候搜了一些文档, 主要提到了要将 `Edit->Project
Settings->Editor` 中的 `Version control mode` 改为 `Visible Meta Files` 以及将
`Asset serialization mode` 改为 `Force Text`. 

仔细调试了一下后, 发现 `Visible Meta Files` 能够使 Unity 在导入工程的时候生成各种
`.meta` 后缀的文件. 而 `Force Text` 则控制着 `*.meta` 文件里的内容格式,
将各种元数据序列化为可读性较好的 `Unity YAML` 格式.

后来测试发现这些 `*.meta` 文件记录着 `Inspector` 面板中的数据, 是 Unity
的各种组件之间的依赖关系的持久化文件.

## _Complete-Game

该目录下包含整个教程完整的示例, 是我学习这个项目的主战场.

### Readme.asset

该文件中记录了 [TutorialInfo/Scripts/Readme.cs](../Assets/TutorialInfo/Scripts/Readme.cs)
类对应的元数据. 进一步探索发现这个资产的 `Inspector` 界面以及顶部工具栏里的 `Tutorial` 菜单都是通过
[TutorialInfo/Scripts/Editor/ReadmeEditor.cs](../Assets/TutorialInfo/Scripts/Editor/ReadmeEditor.cs)
控制的.

到处改一改, 发现 Unity 插件开发相当的敏捷, 不像 IDEA 的插件, 
每次测试插件效果都要重启一个 IDEA 实例. 这个应该也跟编程语言有关系吧.

### Main.unity

主场景, 仔细观察了一下发现在 Camera 上挂载着一个
[Loader](../Assets/_Complete-Game/Scripts/Loader.cs) 脚本. 

脚本内容很简单, 通过 `Inspector` 中设置的属性实例化 
[GameManager](../Assets/_Complete-Game/Scripts/GameManager.cs) 和 
[SoundManager](../Assets/_Complete-Game/Scripts/SoundManager.cs) 对象.

### SoundManager.cs

音效管理器. 由于对象在 Unity 环境中托管, 因此在生命周期函数 `Awake` 
中通过判断静态引用的方式实现了单利设计. 利用 `AudioSource` 类播放本地音频文件, 
为了避免听觉疲劳, 使用了随机数调整音效的高低音.