
## install iTerm2

https://iterm2.com/

## install On-my-zsh

https://ohmyz.sh/#install

## 主题配置

vi ~/.zshrc, 默认的主题已经不错。如果需要可以调整。

## 安装插件

vi ~/.zshrc

```shell
# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)
```
git 插件默认已经安装。

### 安装命令补全插件
参考：https://gist.github.com/dogrocker/1efb8fd9427779c827058f873b94df95

Enabling Plugins (zsh-autosuggestions& zsh-syntax-highlighting)
- Download zsh-autosuggestions by
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
- Download zsh-syntax-highlighting by
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
- nano -/.zshrc find plugins=(git)
- Append zsh-autosuggestions & zsh-syntax-highlighting to plugins() like thisplugins=(git zsh-autosuggestions zsh-syntax-highlighting)
- Reopen terminal

## iTerm2 其他配置

- iTerm2缓冲区设置为无限:
```
iTerm2 --> Setting --> Profiles --> Default --> Terminal --> Scrollback lines --> 选中 Unlimited scrollback (默认非选中)
```
- iTerm2配置选中文字即复制:
```
iTerm2 --> Setting --> General --> Selection --> 选中 Copy to pasteboard on selection (默认选中)
```

## iTerm2光标按照单词快速移动设置
iTerm2之后，发现option+←和option+→这两组快捷键并不能实现光标按照单词快速移动，每次只能一个个字符移动，效率很低。
```
iTerm2 --> Setting --> Profiles --> Default --> 选择“Keys”选项卡
--> 然后可以在Key Mappings看到option+←和option+→这两组快捷键用作了其他功能。
# 设置新的映射
分别修改option+←和option+→的映射如下图所示，选择Action为“Send Escape Sequence”，然后输入“b”和“f”即可。
```
## iTerm2 快捷键
```
command +enter 进入与返回全屏模式
command +t 新建标签
command+w 关闭标签
command+数字 切换标签
command+左右方向键 切换标签
command +f 查找
command+d 水平分屏
command + shift +d 垂直分屏
command+option+方向键 command+[ 或command+] 切换屏幕
command+; 查看历史命令
command+shit+h 查看剪贴板历史
ctrl+u 清除当前行
ctrl +l 清屏
ctrl+a 到行首
ctrl+e 到行尾
ctrl + f/b 前进后退
ctrl+p 上一条命令
```

参考文档: 
- https://www.bilibili.com/video/BV14a4y1F7Ss
- https://cloud.tencent.com/developer/article/2007706
