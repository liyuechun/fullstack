# Level 1. React简易环境安装

欢迎来到「24小时，React+Flux+Redux快速入门」系列教学 :mortar_board: Level 1 ～！
> :bowtie:：Wish you have a happy learning!

## :checkered_flag: 关卡目标

1. 选择一款适合你自己的编辑器
2. React官网
3. 完成主线任务：环境配置、项目初始化


## :triangular_flag_on_post: 选择编辑器

前端工程师常用的编辑器有：

1. [Vim](http://www.vim.org/)
2. [Sublime Text 3](https://www.sublimetext.com/3)
3. [ATOM](https://atom.io/)
4. [Brackets](http://brackets.io/)
5. [Nuclide](http://nuclide.io/)
6. [Webstorm](https://www.jetbrains.com/webstorm/)

> :bowtie:：所谓「工欲善其事，必先利其器」，春哥个人选择的是`ATOM`和`Webstorm`结合使用，***PS：添加春哥微信liyc1215索取Webstorm注册码***！＾＾


## :triangular_flag_on_post: React官网
1. [官网](https://facebook.github.io/react/)

## :triangular_flag_on_post: 主线任务

### 1. 环境配置

下方列出三种安裝方法，挑一个适合你的吧：

1. 简易懒人法：直接从 [官网](https://nodejs.org/) 中下载
2. 进阶识人法：
  1. 安裝 [NVM](https://github.com/creationix/nvm)（Node.js 的版本控管工具；如果你使用 Windows，请至 [NVM for Windows](https://github.com/coreybutler/nvm-windows)）
  2. 使用 NVM 安装 Node.js（安裝指令请见 [NVM 文件](https://github.com/creationix/nvm#usage)）
3. OSX 用户，又刚好装有 [Homebrew](http://brew.sh/)：
  1. 使用 `$ brew install nvm` 安装 NVM，再使用 NVM 下載 Node.js
  2. 使用 `$ brew install node` 直接安裝 Node.js

4. 安装 `create-react-app` 全局命令:直接打开终端，终端输入 `sudo npm install -g create-react-app`安装即可。


> :bowtie:：偷偷跟你讲，春哥选择的是第二种！：）

### 2. 项目初始化

1. 到常用的目录下（如：documents/workspace、desktop）
2. 通过 `create-react-app` 命令创建项目
  1. 切换到桌面
  2. 创建一个空文件夹 `react-quick-study-tutorial`
  3. `cd react-quick-study-tutorial`
  4. `create-react-app HelloWorld`
  5. `cd HelloWorld`
  6. `npm start`
