# Installation

The SDK is one program called `cslash`. It is never installed system-wide: you either download it ready-made (Windows) or build it from source into a folder. Nothing needs Administrator access.

Check that it works with:

```text
cslash version
```

It prints `cslash 1.0.0`.

## Windows

### Ready-made download (easiest)

Download `cslash-windows.zip` from the [Releases page](https://github.com/C-Slash/C-Slash/releases), then in PowerShell:

```powershell
Invoke-WebRequest -Uri https://github.com/C-Slash/C-Slash/releases/latest/download/cslash-windows.zip -OutFile cslash-windows.zip
Expand-Archive cslash-windows.zip -DestinationPath cslash
cd cslash
.\cslash.exe version
```

Windows may show an "Unknown publisher" warning because the program is unsigned. Choose **More info**, then **Run anyway**.

### Build from source without Administrator access

[Scoop](https://scoop.sh) installs tools into your own user folder:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
irm get.scoop.sh | iex
scoop install git cmake mingw ninja
git clone https://github.com/C-Slash/C-Slash.git
cd C-Slash
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build
.\build\cslash.exe version
```

### Build from source with Visual Studio Build Tools

This installer asks for Administrator access.

```powershell
winget install Git.Git
winget install Kitware.CMake
winget install Microsoft.VisualStudio.2022.BuildTools --override "--passive --add Microsoft.VisualStudio.Workload.VCTools --includeRecommended"
```

Close PowerShell and open a new window, then:

```powershell
git clone https://github.com/C-Slash/C-Slash.git
cd C-Slash
cmake -S . -B build
cmake --build build --config Release
.\build\Release\cslash.exe version
```

## Android (Termux)

```bash
pkg install git cmake clang ninja
git clone https://github.com/C-Slash/C-Slash.git
cd C-Slash
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build
./build/cslash version
```

On Android, `dir/default` is `/storage/emulated/0/Download`. Run `termux-setup-storage` once so Termux is allowed to use shared storage.

## Linux (Debian or Ubuntu)

```bash
sudo apt install git cmake g++ ninja-build
git clone https://github.com/C-Slash/C-Slash.git
cd C-Slash
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build
./build/cslash version
```

## macOS

```bash
xcode-select --install
brew install cmake
git clone https://github.com/C-Slash/C-Slash.git
cd C-Slash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build
./build/cslash version
```

`brew` is [Homebrew](https://brew.sh). Install it first if you do not have it.

## Requirements for building

- CMake 3.16 or newer
- A C++17 compiler (GCC, Clang or MSVC)

## Running the program from anywhere (optional)

On Linux, macOS and Termux you can add the build folder to your `PATH`:

```bash
echo 'export PATH="$HOME/C-Slash/build:$PATH"' >> ~/.bashrc
```

Open a new terminal afterwards. On macOS with zsh, use `~/.zshrc` instead of `~/.bashrc`.

On Windows, run the program from its own folder or give its full path.

## Updating

Source builds:

```bash
cd C-Slash
git pull
cmake --build build
```

On Windows with Visual Studio, use `cmake --build build --config Release`.

Windows download: download the newest `cslash-windows.zip` again.

If a build behaves strangely after a big update, delete the build folder and build again:

```bash
rm -rf build
cmake -S . -B build -G Ninja -DCMAKE_BUILD_TYPE=Release
cmake --build build
```

## Uninstalling

The SDK is just a folder, so removing it means deleting that folder.

Android, Linux and macOS:

```bash
cd ~
rm -rf C-Slash
```

Windows:

```powershell
cd ~
Remove-Item -Recurse -Force cslash
Remove-Item cslash-windows.zip
```

If you downloaded or cloned into a different folder, delete that one instead. Both commands are permanent, so check the folder name first.

The build tools (git, cmake and the compiler) are separate programs. Remove them only if nothing else uses them:

- Termux: `pkg uninstall git cmake clang ninja`
- Linux: `sudo apt remove cmake g++ ninja-build`
- Windows with Scoop: `scoop uninstall git cmake mingw ninja`
