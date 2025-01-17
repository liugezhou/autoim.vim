### 结论
自动切换输入法失败。

最近刚入门vim，这篇文章，记录对 vim 切换输入法进行了解的一个过程，以及在了解 vim 插件过程中的一些心路历程。

### 历程

1. 最开始了解 vim 插件是有这么一个需求：

> 每次 Vim 编辑从 insert mode 切换到 normal mode 的时候，若在输入模式中是中文，切回到普通模式的时候，仍旧是中文，这个时候总是需要切换一下输入法到英文，才可以在普通模式下快速输入命令，每次都这么搞的话，非常不方面，那么有没有一个插件可以解决这个问题呢？

2. 然后我在 Github 上搜 `vim输入法切换`，搜到一个仓库[lipingcoding/autoim.vim](https://github.com/lipingcoding/autoim.vim)

> 看到仓库的 star 少，源码也不多，于是我就想那我为什么不自己在本地先去实现一下呢?

3. vim插件开发第一次接触

> 于是先去搜插件开发入门版，看到了这篇文章：[Vim 如何编写插件：Hello World](https://wxnacy.com/2017/12/30/vim-plugin-write-helloworld/) 
> 这篇文章学到的内容是：  
> - source引入vim文件 
> - 在~/.vim/plugin下的.vim文件会自动查找，不用source声明 
> - 函数名和执行命令首字母必须大写
>
> 最后照着这篇文章几分钟后在本地就实现了一个 `Hello World`的入门级插件体验。
4. 然后我又回来看`vim输入法切换`的这个仓库，核心代码就四个文件：
   ![autoim](https://tikolu.net/i/quehu)
> 仓库 fork、clone到本地，查看源代码。  
> 发现`cmd_space.scpt`和`ctrl_space.scpt`这两个文件打不开，也没接触过。且这个仓库只提供了`command+space`和`ctrl + space`两种输入法方式切换，而我本地用的是`caps lock`键。

5. 然后看仓库说明文档，文档最后给出思路来源为[涛叔的博客](https://taoshu.in/vim/vim-auto-im.html) 
接着看涛叔的博客内容，明白了仓库中 autoim.vim 的代码实现思路，以及为什么不能用`caps lock`切换。
这篇文章主要学到的点有：  
> - [AppleScript](https://sspai.com/post/43758#!) 
> - [键盘编码参考](https://eastmanreference.com/complete-list-of-applescript-key-codes) 
> - `scpt`文件打开、编辑方式(Mac 自带的脚本编辑器编辑) 
> - vim 提供的 InsertEnter 和 InsertLeave 两个事件  

6. 回到仓库以`cmd_space.scpt`为例，Mac自带脚本编辑器打开后长这个样子：
    ![](https://tikolu.net/i/feahh)
> 接着看`ctrl_space.scpt`,发现和`cmd_space.scpt`一样，于是将`command`修改为`control`后提交 pull request。

7. 然后使用我自己 fork 的仓库，在`.vimrc`中添加配置：  
    ![](https://tikolu.net/i/fskwl)   
> 插件的状态、安装、更新、删除命令(`PlugStatus` `PlugInstall [name]` `PlugUpdate [name]` `PlugClean(.vimrc中去掉插件说明执行)`) 
> 插件源码路径`~/.vim/plugged/autoim.vim` 
> 这么搞了一波后进行测试，发现第一次从 inset 模式切换到 normal 模式有一定概率可以实现输入法切换，但是第二次就开始快速闪烁，切换不过来了。。。    
> 接着把代码在本地进行修改只监听进入普通模式，试了一下也是不太可以。

8. 这条路走不通，回到5，完全以[涛叔的博客](https://taoshu.in/vim/vim-auto-im.html) 在本地实现。   
- 如6中所示，当点击小三角形进行测试的时候，依然出现的是第二次就不可以正常切换了。   
- 在终端中使用`osascript ~/.vim/liugezhou/ctrlspace.scpt`测试，依然是第二次不能切换问题，
- 目前到这来看，自动切换输入法是失败了。

