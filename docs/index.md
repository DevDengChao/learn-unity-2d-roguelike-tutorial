# 2D Roguelike tutorial

这篇文章是我通过 [2D Roguelike tutorial](https://unity3d.com/learn/tutorials/s/2d-roguelike-tutorial) 
项目在学习 Unity 的使用时留下的记录. 

官方教程中提供的视频来自 YouTube, 国内有搬运工已经把整套视频搬到 
[Bilibili](https://www.bilibili.com/video/av5480978) 了.

## Rules

通过试玩, 我发现了这个小游戏的一些基础的游戏规则:

0. 游戏
0. 地图中的可移动区域为8x8大小, 四周由外墙 (outerWall) 包围.
0. 可移动区域内会生成位置随机, 数量随机的障碍物 (wall), 水果 (food) 和饮料 (soda).
0. 可移动区域内会生成位置随机, 数量逐渐增多的敌人 (enemy).
0. 玩家 (player) 初始生命值 (foodPoint) 为 100, 碰到水果提升 10 点, 碰到饮料
提升 20 点, 被敌人攻击时, 根据敌人类型丢失 10 点或 20 点.
0. 障碍物有 3 点生命值, 被玩家攻击第一次以后会更换材质, 被摧毁后允许玩家和敌人通过.
