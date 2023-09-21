# VSCode-CPP-DevSetup README

## Overview

This repository contains personal Visual Studio Code configuration files for C++ development by `zmr233`. The configuration is designed to be used on **Linux platforms only** and is currently at **version 2.0**.

## Features

### One-Key Compilation

- The standout feature of this configuration is that it allows for two different compilation approaches with a single `F5` press, using both `CMake` and `Make`.

#### Project Structure

- **Root Directory**: Each file in the root directory is treated as an independent project. This means you can have multiple `main()` functions in the root directory, and they will be compiled separately.
- **Subdirectories**: All files in a subdirectory are considered part of the same project, implying that only one `main()` function should exist per subdirectory.

> This setup follows common best practices and allows for multiple projects to co-exist without interference.

#### Executable Files

- Generated executable files will be stored in the `bin` directory with an `_out` suffix.

#### Cleanup

- Use the `make cleanall` command to remove all generated binaries. This must be executed from the root directory.

### MySQL Development Environment (New in Version 2.0)

- Use `InstallMySQL.sh` to automatically install MySQL development tools.

  ```bash
  sudo apt update
  sudo apt install mysql-client
  sudo apt install libmysqlcppconn-dev
  sudo apt install libmysql++-dev
  ```

- Configuration files like `config.h.in` and `.vscode/c_cpp_properties.json` are pre-configured for MySQL.

  ```json
  "includePath": [
      "${workspaceFolder}/**",
      "/usr/include/mysql++",
      "/usr/include/mysql",
      "/usr/include/cppconn"
  ]
  ```

- With these settings, you can now seamlessly integrate MySQL libraries in your C++ projects.

## Installation and Updates

### First-Time Installation

```bash
git clone https://github.com/zmr-233/VSCode-CPP-DevSetup.git
```

### Updating Existing Configuration

```bash
git fetch
git rebase origin/main
# Resolve conflicts manually if any.
```

### MySQL Setup

1. Give execution permission to `InstallMySQL.sh`:

    ```bash
    chmod 777 InstallMySQL.sh
    ```

2. Run `./InstallMySQL.sh`.

3. Perform a first-time build with `F5` to generate `config.h`.

4. Modify `config.h` to enable/disable MySQL libraries.

### Without MySQL

- Remove MySQL paths from `.vscode/c_cpp_properties.json`.
- No need to modify `config.h`; MySQL options are off by default.

## Additional Setup

### GCC and GDB Installation

- To install GCC and GDB, run the following commands:

  ```bash
  sudo apt update
  sudo apt install build-essential
  sudo apt install gdb
  ```

### VSCode Extensions

- Make sure to install the `C/C++` extension provided by Microsoft for IntelliSense and debugging features.

## Known Issues & Bug Fixes

- Avoid having files with the same name in the root and subdirectories.
- Remove corresponding `_out` files when moving or renaming files.
- If you encounter build issues, try deleting the `bin` and `build` directories and rebuild the project using `F5`.

---

For more details, refer to the respective configuration files within this repository. Feel free to contribute and raise issues.

---

# VSCode-CPP-DevSetup 说明文档

## 概览

此仓库包含由 `zmr233` 个人用于 C++ 开发的 Visual Studio Code 配置文件。该配置仅设计用于**仅限 Linux 平台**，并目前处于**2.0 版本**。

## 特性

### 一键编译

- 这个配置的最大特点是它允许使用单一的 `F5` 键进行两种不同的编译方式，既使用了 `CMake` 又使用了 `Make`。

#### 项目结构

- **根目录**：根目录中的每个文件都被视为一个独立的项目。这意味着你可以在根目录中拥有多个 `main()` 函数，并且它们将被独立编译。
- **子目录**：子目录中的所有文件都被视为同一个项目的一部分，这意味着每个子目录下只能有一个 `main()` 函数。

> 这样的设置遵循了常见的最佳实践，并允许多个项目共存而不会相互干扰。

#### 可执行文件

- 生成的可执行文件将以 `_out` 后缀的形式存放在 `bin` 目录下。

#### 清理

- 使用 `make cleanall` 命令来删除所有生成的二进制文件。这必须从根目录执行。

### MySQL 开发环境（2.0 版本新特性）

- 使用 `InstallMySQL.sh` 来自动安装 MySQL 开发工具。

  ```bash
  sudo apt update
  sudo apt install mysql-client
  sudo apt install libmysqlcppconn-dev
  sudo apt install libmysql++-dev
  ```

- 像 `config.h.in` 和 `.vscode/c_cpp_properties.json` 这样的配置文件已预先配置了 MySQL。

  ```json
  "includePath": [
      "${workspaceFolder}/**",
      "/usr/include/mysql++",
      "/usr/include/mysql",
      "/usr/include/cppconn"
  ]
  ```

- 有了这些设置，你现在可以在你的 C++ 项目中无缝集成 MySQL 库。

## 安装和更新

### 首次安装

```bash
git clone https://github.com/zmr-233/VSCode-CPP-DevSetup.git
```

### 更新现有配置

```bash
git fetch
git rebase origin/main
# 如有任何冲突，需手动解决。
```

### MySQL 设置

1. 给 `InstallMySQL.sh` 设置执行权限：

    ```bash
    chmod 777 InstallMySQL.sh
    ```

2. 运行 `./InstallMySQL.sh`。

3. 使用 `F5` 执行首次构建以生成 `config.h`。

4. 修改 `config.h` 以启用/禁用 MySQL 库。

### 不使用 MySQL

- 从 `.vscode/c_cpp_properties.json` 中删除 MySQL 路径。
- 无需修改 `config.h`；MySQL 选项默认是关闭的。

## 额外设置

### GCC 和 GDB 安装

- 要安装 GCC 和 GDB，请运行以下命令：

  ```bash
  sudo apt update
  sudo apt install build-essential
  sudo apt install gdb
  ```

### VSCode 扩展

- 确保安装了由 Microsoft 提供的 `C/C++` 扩展，以获取 IntelliSense 和调试功能。

## 已知问题和错误修复

- 避免在根目录和子目录中有相同名称的文件。
- 在移动或重命名文件时，最好删除相应的 `_out` 文件。
- 当遇到各种构建问题时，尝试删除 `bin` 和 `build` 目录，然后使用 `F5` 重新构建项目。

---

更多详细信息，请参阅本仓库内的相关配置文件。欢迎贡献和提出问题。