# Nailongbot 桌面陪伴机器人 · 组队学习笔记

Datawhale 11 月组队学习《Nailongbot—从零开始组装设计桌面陪伴机器人》的动手记录。

课程 Wiki：<https://nailong.nekokax.com>
Wiki 源码：<https://github.com/lucklyduck/nailongBotWiki>

## 进度

| Task | 内容 | 状态 | 笔记 |
| --- | --- | --- | --- |
| Task01 | 环境准备、组装、舵机归中、整机自检 | ✅ 已打卡 | [task01](task01.md) |
| Task02 | 烧录固件，让屏幕亮起来 | ✅ 已打卡 | [task02](task02.md) |
| Task03 | 配网、绑定设备、舵机校准 | 🟡 笔记就绪 | [task03](task03.md) |
| Task04 | 配置人设，让它有性格 | ⬜ | — |
| Task05 | 二次开发：加动作 / 表情 / 声音 | ⬜ | — |

## 硬件

ESP32-S3 主控 + 圆形屏 + 音频模块 + 4 个 SG90 舵机（两条腿各 2 个关节），锂电池供电，接 xiaozhi.me 做语音交互。

## 环境

- Windows + Catnip Forge（课程提供的集成编译环境，Vibe coding 向的 IDE）
- 备用方案：官方固件网页在线烧录（Chrome / Edge 的 Web Serial）
