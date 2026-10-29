已知bug:
index.html:

window.addEventListener('blur', function () {

scene.pointerLeave();

});

删掉或者注释这一句，否则绳子抓不住(实际上是抓住后会立即call这个回调，松开绳子)

================================================

# Lively-HTML-Programmable-Wallpaper
An interactive, programmable wallpaper powered by Lively and HTML.

2.提供了动态增加拖尾的功能，虽然路径是任意的，但拖尾文件(.js)本身需要实现一些特定的函数！
public interface ITrail
{
    void Create(float x, float y); //每次移动鼠标时调用，x,y是鼠标的坐标。负责生成拖尾数据
    void Update(float dt);  //每帧调用，dt是时间差，秒为单位。负责更新拖尾数据
    void Draw(CanvasRenderingContext2D ctx);  //紧跟着Update调用，负责把拖尾数据绘制出来
    void Clear();  //更换拖尾时调用
}
我相信熟悉C#或者我的人应该知道我在说什么，我就不赘述了。实际上，html似乎有函数名首字母小写的习惯，但我习惯C#规范了，所以沿用了大写。
方法1提供了可视化的增加拖尾的方式，但我不建议大量使用，因为在浏览器里选文件，返回的地址是假的！所以底层是把代码复制到localStorage实现的，大概能存100多个吧...

另一种方式是，打开index.html同目录的TrailConfig.js，里面有示例，实际是一个字典，键和displayName是任意的，className和path必须和拖尾文件一致，path是基于index.html的相对路径或者绝对路径。推荐都放到Trails文件夹下。这样不需要占用localStorage的空间

3.对lively源代码的一些小修改，主要是把windows消息转发到Web那一块
*把原来的鼠标钩子替换成了低级鼠标钩子，解决点击桌面空白处不能转发鼠标点击信号的问题(也可能是我的电脑的问题...)
修改文件: RawInputMsgWindow.xaml.cs
*不再用钩子发鼠标移动消息，而是主动轮询。解决点击桌面空白处后，不能转发鼠标移动信号的问题
修改文件: WinDesktopCore.cs
*新增了鼠标中键的转发消息
修改文件: RawInputMsgWindow.xaml.cs
*模拟沙盒不再使用随机路径，而是固定路径。解决无法使用localStorage反序列化数据的问题。修改函数:WebView2UserDataFactory.GetTempUserDataFolder
*未解决问题: 键盘消息还是无法转发，不管是官方使用的钩子，还是低级键盘钩子

我之后去提个issue，官方改完之后，我再改用官方版本

4.壁纸的背景是可以替换的，但对路径和命名有要求。必须和index.html文件同目录，并且命名为bg.png

5.因为我的惯用语言是C#，所以实在不熟悉html和js的语法和工具流。大部分代码是dsh写的，我提供了一些架构和接口的建议，以及对各模块的组装(组装代码在index.html)。架构来源于我的C#和Unity经验，所以可能和html的主流架构不符

6.有一个烟花方案没有使用，因为和背景不搭，也比较吃性能。但是效果还可以，我放在Unused里了

7.交互方法
*鼠标中键喊出控制面板，再点击则关闭。
*控制面板中有一个"拖尾类型"下拉框，可以更换拖尾，添加拖尾，删除拖尾。注意:自带的(写在TrailSetting)拖尾无法删除，可以删除的拖尾，把鼠标悬浮在其上，右侧会有删除按钮
*蜡烛可拖拽，鼠标滑动吹灭，再点击复燃。蜡烛位置不序列化，如果需要，可以改源码，或者稍等？
*气球点击绳子是拖拽，点击"球体"是炸开。不启用气球不会让画面中的气球立即消失！而是不再自动生成气球

8.创作动机
前段时间写游戏，越写越烦。一个人写还真是，怎么说呢，是孤独吗？然后我想着好久没换壁纸了，deepseek就告诉了我可交互壁纸这个东西。啊？壁纸也可以交互？然后我就接触了Lively，开始了这一段旅程

9.然后壁纸上的角色是前园雾架，侦探，出自箱庭逻辑。什么？没看出来？因为是ai生成的嘛，其实我更喜欢这张图的氛围，我感觉这张图的意境很好！

10.然后感谢Lively！感谢梁圣及其deepseek！我真的很喜欢lively，如果我的做法侵害了原作者的权益，请通知我！我会立即删掉这个"魔改版"。最后祝雾架生日快乐！(虽然已经过去很多天了...)祝大家玩得开心！
