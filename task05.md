# Task05 · 二次开发：加一个自己的动作

> 目标：让奶龙学会一个原来没有的动作。参考 <https://nailong.nekokax.com/docs/dev/action>、[源码结构](https://nailong.nekokax.com/docs/dev/source)

## 一、先理解一条链路

```
你说「来个某某动作」
  → 模型调用 self.otto.action(action="xxx", steps, speed, ...)
  → MCP 回调 QueueAction(...) 把动作交给后台任务
  → 动作任务调用 otto_movements.cc 的对应方法
  → 执行完归位
```

关键认知：**动作不是「一个动作一个 MCP 工具」**，而是统一走 `self.otto.action`，用参数里的 `action` 名字选。所以加动作 = 让模型认识一个新名字 + 让代码认识一个新名字。

## 二、路线 A：30 分钟版（先加别名，跑通链路）

改 `main/boards/otto-robot/otto_controller.cc`，在解析 `action` 的分支里、紧挨 `swing` 加一段：

```cpp
else if (action == "my_swing") {
    QueueAction(ACTION_SWING, steps, speed, 0, amount);
    return true;
}
```

这只是给现有「摇摆」起个新名字，复用真实队列和轨迹——目的是先把「模型参数 → 队列 → 舵机」这条路跑通，同时把工具描述也加上 `my_swing`，否则模型不会用。

适合时间紧、想先看到「我说了个新词，它真的做出来了」的情况。

## 三、路线 B：真写一条新轨迹

| 改哪里 | 做什么 |
| --- | --- |
| `otto_movements.h/.cc` | 加新动作方法，复用现有插值/振荡 |
| `otto_controller.cc` 动作枚举 | 加一个不重复的编号 |
| 动作任务分支 | 调新方法，沿用归位处理 |
| `action` 参数分支 + 工具描述 | 新名字映射到新编号 |
| 取消检查 | 照 `IsCancelled()`，循环/插值/长等待都要能中断 |

从 `Swing`、`Bend` 这种小改动改起，别一上来写复杂循环。

## 四、编译与实测

1. 按 [编译奶龙固件](https://nailong.nekokax.com/docs/flash/build) 构建（`scripts/build_nailong.py`）并烧录
2. **机器人架空**，先少次数、小幅度、慢速度
3. 看日志里模型实际传的参数，再核对方向和归位
4. 中途喊停，确认停止后不会继续写旧轨迹

参数范围：`steps` 1～100、`speed` 100～3000（毫秒，越大越慢）、`amount` 0～170。**范围合法 ≠ 机械安全。**

## 五、常见问题

| 现象 | 检查 |
| --- | --- |
| 模型说动作不支持 | 工具描述和名称分支是否都改了 |
| 编译过但没动作 | 编号、任务分支、引脚、队列是否对应 |
| 停止后又继续动 | 循环/插值/等待里漏了取消检查 |
| 新动作容易摔 | 先查校准，再缩小幅度、降速 |

⚠️ 语音「停止」不是机械急停。听到持续嗡鸣、看到碰撞堵转，直接断电。

## 六、下一步

六个 Task 走完，剩下的就是把改过的东西整理成一篇帖子（加分项：晒出你的奶龙）。
