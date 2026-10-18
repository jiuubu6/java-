# 不同开发环境配置 Java 环境（JDK）

> 
> 核心：Java 开发环境 = **JDK**（Java 开发工具包，包含 JRE 运行环境 + javac 编译器）
> 两个关键配置：**安装 JDK** + **配置环境变量（JAVA_HOME、Path）**
> 检查是否成功命令（cmd / 终端通用）

```
java -version
javac -version
```

> 
> 两个命令都能输出版本号才算配置完成；只有`java`能运行说明只装了 JRE，没有编译器`javac`。

## 一、Windows 系统（记事本 / VSCode / IDEA 通用）

### 1. 下载 JDK

推荐初学：**JDK8 或者 JDK17（长期支持 LTS 版本）**
Oracle 官网 / OpenJDK / Amazon Corretto 都可以，下载 Windows x64 安装包。

### 方式 A：安装包一键安装（简单）

1. 双击`.exe`安装，记住安装路径，例如：`C:\Program Files\Java\jdk17`
2. 打开【此电脑】右键 → 属性 → 高级系统设置 → 环境变量
3. 在**系统变量**新建：
   - 变量名：`JAVA_HOME`
   - 变量值：JDK 安装目录 `C:\Program Files\Java\jdk17`
4. 找到系统变量里的`Path`，编辑 → 新建，填入：`%JAVA_HOME%\bin`
5. 全部确定保存，**关闭旧 cmd 窗口重新打开**，输入`java -version`验证

### 方式 B：绿色压缩包（zip 免安装）

1. 解压 zip 到无中文、无空格路径，例如 `D:\jdk17`
2. 同上配置`JAVA_HOME`和 Path，步骤一样。

> 
> ✅ Windows 下 IDEA、VSCode 会优先读取系统 JAVA_HOME；
> VSCode 额外需要安装插件：Extension Pack for Java。

---

## 二、MacOS 系统（终端 + IDEA / VSCode）

### 方式 1：Homebrew 安装（推荐）

打开终端：

```
# 安装JDK17示例
brew install openjdk@17
```

安装完成后，配置环境变量

```
# 编辑配置文件
vim ~/.zshrc
```

在文件末尾添加（路径根据安装提示修改）

```
export JAVA_HOME="/opt/homebrew/opt/openjdk@17"
export PATH="$JAVA_HOME/bin:$PATH"
```

保存退出，生效配置：

```
source ~/.zshrc
```

验证：

```
java -version
javac -version
```

### 方式 2：dmg 安装包

下载 mac dmg 安装，同样配置`JAVA_HOME`写入`.zshrc`。

---

## 三、Linux（Ubuntu 为例，服务器 / 虚拟机）

```
# 更新软件源
sudo apt update
# 安装JDK17
sudo apt install openjdk-17-jdk
```

查看 java 安装路径

```
update-alternatives --list java
```

配置环境变量

```
sudo vim /etc/profile
```

末尾添加：

```
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
```

生效：

```
source /etc/profile
```

---

# 四、各个开发工具内单独选择 JDK（重点！）

> 
> 系统环境变量是全局默认 JDK，**IDE 可以单独指定项目 JDK，不和系统共用**

## 1. IDEA（IntelliJ IDEA）

1. File → Project Structure（快捷键 `Ctrl+Alt+Shift+S`）
2. Project Settings → Project
   - Project SDK：选择已安装的 JDK；没有就点`Add SDK`，选中本地 JDK 文件夹
   - Language level：对应 JDK 版本（JDK17 选 17）
3. 模块：Modules，同样选择 SDK

> 
> IDEA 自带内置 JDK，但是**写 Java 代码建议自己配置独立 JDK**

## 2. VSCode

1. 安装插件：Extension Pack for Java
2. 快捷键 `Ctrl+Shift+P` → 输入 `Java: Configure Java Runtime`
3. 点击 `Add Java Runtime`，选择本地 JDK 目录，设置为项目使用的 JDK

> 
> VSCode 会自动读取`JAVA_HOME`，多个 JDK 可以在这里切换版本

## 3. Eclipse

1. Window → Preferences
2. Java → Installed JREs → Add
3. 选择 Standard VM，Directory 选中 JDK 根目录，完成添加
4. 勾选设为默认 JRE；Compiler 里设置编译版本

## 4. 纯记事本（无 IDE，只用 cmd / 终端）

**完全依赖系统环境变量 JAVA_HOME + Path**，没有单独配置，配置好系统环境变量直接用`javac`编译。

---

# 五、常见踩坑

1. ❌ 路径带中文 / 空格：`D:\软件\jdk` 会报错，改用纯英文路径
2. ❌ 配置环境变量后 cmd 不生效：**一定要关闭 cmd 重新打开**
3. ❌ `java`能用，`javac`不是内部命令：装成 JRE 了，必须装 JDK
4. ❌ 多个 JDK 共存：系统`JAVA_HOME`控制默认；IDE 项目可以单独选不同版本
5. ❌ Mac/Linux 配置完不生效：没执行`source`命令，或者配置文件写错

---

# 快速总结

- 系统层面：安装 JDK → 配置`JAVA_HOME`、`Path` → 终端验证
- IDE 层面：可以独立指定项目 JDK，不受系统全局 JDK 限制

如果你需要，我可以给你写一份**Windows 一键配置 JAVA_HOME 的批处理脚本**，或者带你一步步配置 VSCode Java 环境，直接跑前面 HelloWorld 代码。
