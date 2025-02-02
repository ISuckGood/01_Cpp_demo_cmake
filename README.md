# Using GCC on macOS (Apple Silicon) with VS Code and CMake

This guide outlines the steps to set up a C++ development environment on macOS (Apple Silicon) using GCC as the compiler, VS Code as the IDE, and CMake as the build system.

## Prerequisites

*   **VS Code:** Download and install Visual Studio Code from [https://code.visualstudio.com/](https://code.visualstudio.com/).

## Installation Steps

1.  **Install Homebrew (if you don't have it):**
    *   Open Terminal and run:
        ```bash
        /bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
        ```

2.  **Install CMake and GCC:**
    *   Open Terminal and run:
        ```bash
        brew update
        brew install cmake gcc
        ```

3.  **Install VS Code Extensions:**
    *   Open VS Code.
    *   Go to the Extensions Marketplace (Ctrl+Shift+X or Cmd+Shift+X).
    *   Install the following extensions:
        *   **C/C++** (by Microsoft) - Provides IntelliSense, debugging, and code browsing.
        *   **CMake Tools** (by Microsoft) - Provides integration with CMake.

4.  **Create a Simple Project (Optional):**
    *   Create a new folder for your project.
    *   Inside the folder, create a simple `main.cpp` file (you can use a "Hello, World!" example).
    *   Create a `CMakeLists.txt` file to manage the build process. Here's a minimal example:

        ```cmake
        cmake_minimum_required(VERSION 3.10)
        project(MyProject)

        set(CMAKE_CXX_STANDARD 17)
        set(CMAKE_CXX_STANDARD_REQUIRED ON)

        add_executable(MyProject main.cpp)
        ```

5.  **Configure VS Code for GCC:**

    *   Open the Command Palette in VS Code (Ctrl+Shift+P or Cmd+Shift+P).
    *   Type "C/C++: Edit Configurations (JSON)" and select it. This will open the `.vscode/c_cpp_properties.json` file.
    *   Add a new configuration for GCC. You might need to replace the `compilerPath` with the correct path to your Homebrew GCC installation if it is not automatically detected correctly in the next step. You can find the path by running `which gcc-14` (or the relevant version) in your terminal.

        ```json
        {
            "configurations": [
                {
                    "name": "Mac GCC",
                    "includePath": [
                        "${workspaceFolder}/**"
                    ],
                    "defines": [],
                    "compilerPath": "/opt/homebrew/bin/g++-14",
                    "cStandard": "c17",
                    "cppStandard": "c++17",
                    "intelliSenseMode": "macos-gcc-arm64",
                    "configurationProvider": "ms-vscode.cmake-tools"
                }
            ],
            "version": 4
        }
        ```

6.  **Select the GCC Kit in CMake Tools:**

    *   Open the VS Code Command Palette.
    *   Type "CMake: Scan for Kits" and select it. This will make CMake Tools look for available compilers.
    *   Open the Command Palette again.
    *   Type "CMake: Select a Kit" and select it.
    *   Choose the kit that corresponds to your Homebrew GCC installation (e.g., "GCC 14.2.0 aarch64-apple-darwin24").

7.  **Configure and Build:**

    *   Open the VS Code Command Palette.
    *   Type "CMake: Configure" and select it to generate the build files using your selected kit.
    *   Open the VS Code Command Palette.
    *   Type "CMake: Build" and select it.

8.  **Verify Compiler Path (Optional):**

    *   After configuring, you can check the `build/compile_commands.json` file (if generated by CMake) to ensure that the correct compiler path is being used. Look for the `"command"` field, which should point to your Homebrew GCC.

## Troubleshooting

*   **Compiler Not Found:** If CMake Tools cannot find your GCC installation, make sure that Homebrew's `bin` directory is in your `PATH` environment variable and/or verify that the `compilerPath` in your `c_cpp_properties.json` is correct. You may have to add `export PATH="/opt/homebrew/bin:$PATH"` to your shell configuration (`.zshrc` or `.bashrc`) if it is not already there. Then do source `~/.zshrc` or source `~/.bashrc`.
*   **IntelliSense Errors:** If you encounter IntelliSense errors, try reloading the VS Code window (Cmd+Shift+P or Ctrl+Shift+P, then type "Reload Window").

## Notes

*   The specific version numbers of GCC (e.g., `gcc-14`) might vary depending on when you installed it. Adjust the paths accordingly.
*   The `intelliSenseMode` should match your compiler and architecture. `macos-gcc-arm64` is for Apple Silicon using GCC. Use `macos-gcc-x64` for Intel-based Macs.
*   It is recommended to add to your `CMakeLists.txt` file these lines to ensure that you are using at least c++17:

    ```cmake
    set(CMAKE_CXX_STANDARD 17)
    set(CMAKE_CXX_STANDARD_REQUIRED ON)
    ```