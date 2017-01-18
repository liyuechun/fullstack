# Level 16. 用元件思维设计应用程序

欢迎来到「24小时，React+Flux+Redux快速入门」系列教学 :mortar_board: Level 16 ～！
> :bowtie:：Wish you have a happy learning!


## :checkered_flag: 关卡目标

1. 完成主线任务：
  1. 列出 TodoApp 的功能清单
  2. 使用「元件思维」设计 TodoApp
2. 习得心法：打通任督二脉，了解「**元件思維**」


## :triangular_flag_on_post: 主线任务

### 1. 列出 TodoApp 的功能清单

相信大家都有用过市面上的 TodoApp，一个简单的 TodoApp 会有以下功能：

1. 列出所有待办项目
2. 提示待办数量
3. 新增待办法项目
4. 编辑待办项目
5. 刪除待办项目
6. 切换项目处理状态

根据以上功能，春哥为大家制定了第一版的TodoApp

![TodoApp markup](http://ojwkz03vq.bkt.clouddn.com/diyiban_xiaoguotu.png)

### 2. 使用元件思维，设计 TodoApp

> :neckbeard:：骚年，我看你天资聪颖，是万中选一的前端人，我这有..[武功秘笈](http://www.kongyixueyuan.com)，替你打通元件思维的任督二脉，你斟酌看看 :lollipop:

将上一步的 UI 割分成多个元件，并且替它们取上名称：

![TodoApp components](http://ojwkz03vq.bkt.clouddn.com/diyiban_xiaoguotu-quming.png)

> :bowtie:：在这一阶段我们必须尽可能得让元件可以**重复利用**；思考逻辑是这样的 - 因为 TodoList 中每一行待办项目的 UI 都是相同的，仅显示的资料不一样，所以我们将待办项目抽象成 TodoItem 元件。
