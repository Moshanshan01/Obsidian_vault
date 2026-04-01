**STM32CubeMX 生成的代码默认使用 2 个空格缩进**，目前官方界面上**没有直接提供修改缩进为 4 个空格的选项**。

但你仍然有几种常用且有效的解决办法，按推荐程度排序如下：

### 方法 1（最推荐）：用编辑器的「格式化替换」功能一次性改（最快）

几乎所有现代 IDE 都支持这个操作：

| 编辑器              | 操作方式（把 2空格 转成 4空格）                                                                                   | 快捷键参考     |
| ---------------- | ---------------------------------------------------------------------------------------------------- | --------- |
| VS Code          | 1. 全选代码<br>2. 按 `Ctrl+Shift+P` → 输入 `Convert Indentation to Spaces`<br>3. 选 `Indent Using Spaces: 4` | 通常直接有效    |
| VS Code          | 更暴力方法：查找替换（`Ctrl+H`）<br>查找：`^(\s{2})+`（正则）<br>替换：`$1$1`（或直接写 4 个空格）                                  | 推荐正则方式    |
| Keil / MDK       | 编辑 → 高级 → 替换（勾选正则表达式）<br>查找：`^  `（两个空格）<br>替换：`    `（四个空格）                                           | 手动点「全部替换」 |
| CubeIDE          | 右键代码区域 → Source → Format<br>或：Ctrl+Shift+F（格式化）<br>但更推荐先改成 4 空格再格式化                                  | 先替换再格式化   |
| CLion / IntelliJ | 1. 全选 → Code → Convert Indents → To Spaces<br>2. 或直接查找替换两个空格 → 四个空格                                  | 很方便       |
| Notepad++        | 查找 → 替换 → 勾选「正则表达式」<br>查找：`^(\s{2})+`<br>替换：`\t\t`（或直接四个空格）                                          | 适合临时处理文件  |

### 方法 2：修改 CubeMX 工程模板（适合长期使用同一套风格的人）

路径（Windows为例）：

```
C:\Users\你的用户名\STM32Cube\Repository\
   找到你使用的 MCU 系列文件夹（例如 STM32Cube_FW_F4_V1.28.0）
   进入 \Middlewares\Third_Party 或 \Drivers 等目录
   找到 .ioc 文件夹旁边的生成模板（通常在 Project Manager → Code Generator 里能看到部分线索）
```

但目前（2025年）更常见做法是：

1. 先让 CubeMX 生成一次代码
2. 把生成的 Src / Inc 目录里的 .c/.h 文件全部复制出来
3. 用你喜欢的编辑器把缩进改成 4 空格（用上面方法 1）
4. 以后每次重新生成代码后，都**只覆盖**外设初始化部分（保持 main 里的 USER CODE 区域不变）
5. 再对整个工程执行一次「2空格转4空格」的批量替换

### 方法 3：使用外部格式化工具（自动化程度最高）

推荐组合：

```
clang-format（配合 .clang-format 文件）  ←  目前社区最推荐
astyle
uncrustify
```

示例 .clang-format 文件内容（只改缩进宽度）：

```yaml
BasedOnStyle: LLVM
IndentWidth: 4
TabWidth: 4
UseTab: Never
ColumnLimit: 120
```

然后在 VS Code / CubeIDE 里配置保存时自动格式化，或者写个小脚本在生成代码后自动跑一次。

### 总结推荐流程（最省事）

1. 用 VS Code 打开整个工程
2. 安装 C/C++ 扩展
3. 按 `Ctrl+Shift+P` → `Preferences: Open Settings (JSON)`
4. 加入：

```json
"C_Cpp.clang_format_fallbackStyle": "{ BasedOnStyle: Google, IndentWidth: 4, TabWidth: 4 }"
```

5. 以后每次 CubeMX 生成代码后：
   - 全选所有 .c/.h 文件
   - 右键 → Format Document (Shift+Alt+F)

基本上就能保持 4 空格风格了。

祝你代码看着舒服～