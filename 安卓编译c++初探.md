## 安卓编译c++初探

### 安卓示例项目

- 安卓示例项目使用ndk（native development kit）作为开发工具，主要方式即使用jni（Java native interface) 作为java与C++之间的胶水层，从而实现java和c++代码的互相调用。
- java调用C++代码实现方式：
  - 主要思路为C++编译出动态链接（使用cmake）库供java层调用
  - java代码中载入c++动态库，通常都是在static语句块中调用System.loadLibrary("xxx")实现
  - java代码中使用native关键字声明，不需要实现
  - c++代码中实现该声明函数，如何对应暂时不理解，大致上时通过名称对应，需要前面加上路径名，用下划线隔开==，这里规则有点混论，暂时没弄明白

### 从Neox引擎查起

- 思路一，搜索System.loadLibrary函数的调用
  - 在Launcher.java.in文件内发现有使用，看了一下该文件，主要都是patch相关的操作，包括一些系统参数的获取，以及patch界面的显示等等
  - 此外，on_create中发现了GLSurfaceView的初始化，似乎与游戏主题无关？
  - 最后在startgame函数内发现了新创建了Client这个Activity，这个是继承自NeoxClient的