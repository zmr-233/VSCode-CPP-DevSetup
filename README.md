# VSCode-CPP-DevSetup

## Description

This repository provides a streamlined setup for C++ development in Visual Studio Code. It emphasizes:
- Out-of-source builds using `CMake` and `make`.
- Individual executable generation for each `.cpp` file.
- Keeping the workspace clean by avoiding unnecessary files and directories in the root.

## Setup

1. Clone this repository:
```bash
   git clone https://github.com/your-username/VSCode-CPP-DevSetup.git
   cd VSCode-CPP-DevSetup
```

2. Ensure you have CMake and make installed:
```bash
  cmake --version
  make --version
```
3. Open the project in Visual Studio Code.

4. Press F5 in any .cpp file to compile and debug.

## Features
- Out-of-Source Builds: All CMake configurations and build files are generated inside the build directory.
- Individual Executables: Each .cpp file generates its own executable in a dedicated bin directory, either in the root or inside the source's directory.

## Known Issues
**Unwanted Directories**: After running CMake, you might notice directories named CompilerIdC/bin and CompilerIdCXX/bin being created. These directories are not essential to the main project and are an artifact of CMake's internal workings. At the moment, we're unaware of a way to prevent their creation. If these directories pose an issue, they can be safely deleted.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.
Just copy and paste this content to your **README.md** file to include the new section on known issues.