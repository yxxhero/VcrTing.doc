<img src="https://github.com/VcrTing/VcrTing.doc/blob/master/SAVE/imgs/X/1.jpg?raw=true" width="100%">

### 安装 Flutter
1. 先配置环境变量：vim ~/.bash _profile  
    `export PUB_HOSTED_URL=https://pub.flutter-io.cn`  
    `export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn`  

2. 下载并解压flutter到任意文件夹，[Official](https://flutter.dev/docs/development/tools/sdk/releases?tab=macos#macos) or [Github](https://github.com/flutter/flutter/releases)。  

3. 配置解压的flutter到环境变量：vim ~/.bash_profile  
    `export PATH='pwd'/flutter/bin:$PATH`  

4. 生效环境变量并且测试flutter环境：  
    生效：source ~/.bash_profile  
    查看隐藏文件：ls -a  
    测试flutter：flutter doctor

### 安装 依赖
1. 下载安装Xcode，到App store 下载安装 Xcode  

2. 下载安装[Android Studio](http://www.android-studio.org/)  

3. 打开Android Studio 安装SDK  

4. 运行flutter doctor，同意Android License  

5. 打开Xcode 安装Components  

6. 安装cocoapods: sudo gem install cocoapods  

7. 同意Xcode License：  
    `sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer`  
    `sudo xcodebuild -license`  

8. 测试Xcode 模拟器：open -a Simulator

9. VS Code 安装 flutter 拓展

10. Android Studio 安装 Flutter Plugin，在Preference里面

11. [体验](https://flutterchina.club/get-started/test-drive/#vscode)

### 连接设备  

<br/>
<p align='right'>💗</p>