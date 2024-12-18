在 Ubuntu 中切换 Java 版本可以通过 `update-alternatives` 命令来实现。以下是具体步骤：

1. **查看已安装的 Java 版本**：
   首先，你需要确认系统中安装了哪些 Java 版本。可以使用以下命令查看：
   ```bash
   update-alternatives --config java
   ```

2. **切换 Java 版本**：
   执行上面的命令后，你会看到一个列表，其中包含了系统中所有可用的 Java 版本。每个版本前面都有一个编号。输入你想使用的 Java 版本对应的编号，然后按 Enter 键即可切换。

3. **验证切换结果**：
   切换完成后，可以通过以下命令验证当前正在使用的 Java 版本：
   ```bash
   java -version
   ```

如果你需要切换 `javac`（Java 编译器）的版本，可以使用类似的命令：
```bash
update-alternatives --config javac
```

确保在执行这些命令时具有管理员权限（使用 `sudo`），以便对系统设置进行更改。