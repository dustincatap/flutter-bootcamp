# Setting Up Flutter Development Environment

Getting your development environment properly configured i### Installation Steps

1. **Download and Install VS Code**
   - Visit [code.visualstudio.com](https://code.visualstudio.com/)
   - Download and install for your operating system

2. **Install Flutter Extension**
   - Open VS Code
   - Click on the Extensions icon in the sidebar (or press `Ctrl+Shift+X` / `Cmd+Shift+X`)
   - Search for "Flutter"
   - Click "Install" on the official Flutter extension by Dart Code
   - This will also install the Dart extension automatically

3. **Verify Extension Installation**
   - Press `Ctrl+Shift+P` / `Cmd+Shift+P` to open the Command Palette
   - Type "Flutter" and you should see Flutter-related commands

## System Requirements

Before installing Flutter, ensure your system meets the minimum requirements:

### Windows
- Operating System: Windows 10 or later (64-bit)
- Disk Space: 1.64 GB (does not include disk space for IDE/tools)
- Tools: Windows PowerShell 5.0 or newer, Git for Windows

### macOS
- Operating System: macOS 10.14 (Mojave) or later
- Disk Space: 2.8 GB (does not include disk space for IDE/tools)
- Tools: Bash, curl, git, mkdir, rm, unzip, which, zip

### Linux
- Operating System: 64-bit distribution
- Disk Space: 600 MB (does not include disk space for IDE/tools)
- Tools: bash, curl, file, git, mkdir, rm, unzip, which, xz-utils, zip

## Install Flutter SDK

### Windows Installation

1. **Download Flutter SDK**
   - Visit the [Flutter SDK download page](https://docs.flutter.dev/get-started/install/windows)
   - Download the latest stable release (ZIP file)

2. **Extract the ZIP file**
   - Extract the downloaded ZIP file to a desired location (e.g., `C:\src\flutter`)
   - **Important**: Do not install Flutter in directories that require elevated privileges (e.g., `C:\Program Files\`)

3. **Update your PATH**
   - Search for "env" in Windows search and select "Edit environment variables for your account"
   - Under "User variables", find the `Path` entry
   - Click "Edit" and add a new entry with the full path to `flutter\bin` (e.g., `C:\src\flutter\bin`)
   - Click "OK" to save

4. **Verify Installation**
   - Open a new Command Prompt or PowerShell window
   - Run: `flutter --version`
   - You should see Flutter version information

### macOS Installation

1. **Download Flutter SDK**
   - Visit the [Flutter SDK download page](https://docs.flutter.dev/get-started/install/macos)
   - Download the latest stable release for your chip (Intel or Apple Silicon)

2. **Extract and Install**
   ```bash
   cd ~/development
   unzip ~/Downloads/flutter_macos_[version].zip
   ```

3. **Update your PATH**
   - Open or create `~/.zshrc` (or `~/.bash_profile` if using bash)
   - Add the following line:
     ```bash
     export PATH="$PATH:`pwd`/flutter/bin"
     ```
   - Refresh your current terminal:
     ```bash
     source ~/.zshrc
     ```

4. **Verify Installation**
   ```bash
   flutter --version
   ```

### Linux Installation

1. **Download Flutter SDK**
   - Visit the [Flutter SDK download page](https://docs.flutter.dev/get-started/install/linux)
   - Download the latest stable release

2. **Extract and Install**
   ```bash
   cd ~/development
   tar xf ~/Downloads/flutter_linux_[version].tar.xz
   ```

3. **Update your PATH**
   - Open or create `~/.bashrc` or `~/.zshrc`
   - Add the following line:
     ```bash
     export PATH="$PATH:$HOME/development/flutter/bin"
     ```
   - Refresh your current terminal:
     ```bash
     source ~/.bashrc
     ```

4. **Verify Installation**
   ```bash
   flutter --version
   ```

## Set Up IDE (Visual Studio Code)

Flutter development works best with a proper IDE. We'll be using Visual Studio Code, a lightweight and fast code editor with excellent Flutter support.

### Installation Steps

1. **Download and Install VS Code**
   - Visit [code.visualstudio.com](https://code.visualstudio.com/)
   - Download and install for your operating system

2. **Install Flutter Extension**
   - Open VS Code
   - Click on the Extensions icon in the sidebar (or press `Ctrl+Shift+X` / `Cmd+Shift+X`)
   - Search for "Flutter"
   - Click "Install" on the official Flutter extension by Dart Code
   - This will also install the Dart extension automatically

3. **Verify Extension Installation**
   - Press `Ctrl+Shift+P` / `Cmd+Shift+P` to open the Command Palette
   - Type "Flutter" and you should see Flutter-related commands

### Key Features in VS Code

- **Hot Reload**: Automatically enabled when running Flutter apps
- **Widget Editing Assists**: Quick fixes and refactoring options
- **Dart DevTools**: Integrated debugging and profiling tools
- **Emulator Support**: Launch and manage emulators directly from VS Code
- **IntelliSense**: Code completion and suggestions
- **Flutter Outline**: Visual representation of widget tree

### Useful VS Code Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Command Palette | `Ctrl+Shift+P` | `Cmd+Shift+P` |
| Quick Open File | `Ctrl+P` | `Cmd+P` |
| Toggle Terminal | `Ctrl+` | `Cmd+` |
| Go to Definition | `F12` | `F12` |
| Format Document | `Shift+Alt+F` | `Shift+Option+F` |
| Save All | `Ctrl+K S` | `Cmd+K S` |

## Flutter Doctor

Flutter Doctor is a command-line tool that checks your environment and displays a report of the status of your Flutter installation. It helps identify missing dependencies and configuration issues.

### Running Flutter Doctor

Open your terminal or command prompt and run:

```bash
flutter doctor
```

### Understanding the Output

Flutter Doctor will check for:

1. **Flutter SDK**: Verifies Flutter installation
2. **Android toolchain**: Checks Android SDK and tools
3. **Xcode** (macOS only): Verifies Xcode installation for iOS development
4. **Chrome**: Checks for web development support
5. **Visual Studio Code**: Verifies IDE installation and plugins
6. **Connected devices**: Shows available devices for testing

#### Sample Output

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, 3.16.0, on Microsoft Windows [Version 10.0.22621.2715], locale en-US)
[✓] Windows Version (Installed version of Windows is version 10 or higher)
[!] Android toolchain - develop for Android devices (Android SDK version 33.0.0)
    ✗ cmdline-tools component is missing
[✓] Chrome - develop for the web
[✓] Visual Studio Code (version 1.84.2)
[✓] Connected device (2 available)
[✓] Network resources

! Doctor found issues in 1 category.
```

### Understanding the Symbols

- **[✓]** Green checkmark: Component is properly installed and configured
- **[!]** Yellow exclamation: Component is installed but has minor issues
- **[✗]** Red X: Component is missing or has critical issues

### Common Issues and Solutions

#### Issue: Android toolchain not found

**Solution:**
1. Download and install Android Studio from [developer.android.com/studio](https://developer.android.com/studio)
2. Open Android Studio and go to **SDK Manager**
3. Install Android SDK and necessary build tools
4. Run `flutter doctor --android-licenses` to accept licenses

**Note**: While we use VS Code for development, Android Studio is still needed to install the Android SDK and tools required for Android app development.

#### Issue: Xcode not configured (macOS)

**Solution:**
1. Install Xcode from the Mac App Store
2. Run: `sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer`
3. Run: `sudo xcodebuild -runFirstLaunch`
4. Accept Xcode license: `sudo xcodebuild -license`

#### Issue: VS Code Flutter extension not detected

**Solution:**
1. Open VS Code
2. Install the Flutter extension from the marketplace
3. Run `flutter doctor` again

#### Issue: cmdline-tools component missing

**Solution:**
1. Open Android Studio (if installed for Android SDK management)
2. Go to **SDK Manager > SDK Tools**
3. Check "Android SDK Command-line Tools"
4. Click "Apply" to install

Alternatively, you can install command-line tools directly from the Android developer website.

### Verbose Output

For detailed information about each check, run:

```bash
flutter doctor -v
```

This provides additional details including:
- Exact version numbers
- Installation paths
- Available devices
- Detailed error messages

### Accepting Android Licenses

If you see license-related issues, run:

```bash
flutter doctor --android-licenses
```

Press `y` to accept each license when prompted.

## Verifying Your Setup

After completing the installation and setup, verify everything is working:

### 1. Check Flutter Version

```bash
flutter --version
```

### 2. Run Flutter Doctor

```bash
flutter doctor
```

Resolve any issues indicated by `[!]` or `[✗]`.

### 3. List Available Devices

```bash
flutter devices
```

This shows all available devices (emulators, simulators, physical devices, web).

### 4. Create a Test Project

```bash
flutter create test_app
cd test_app
flutter run
```

If everything is configured correctly, you should see a list of available devices and be able to run the default Flutter app.

## Next Steps

Once your environment is set up:

1. ✅ Verify all Flutter Doctor checks pass (or have acceptable warnings)
2. ✅ Familiarize yourself with your chosen IDE
3. ✅ Set up an emulator or connect a physical device
4. ✅ Create and run a sample Flutter project
5. ✅ Explore Flutter DevTools for debugging

You're now ready to start building Flutter applications! 🎉

---

## References and Further Reading

### Installation Guides
- [Flutter Installation - Windows](https://docs.flutter.dev/get-started/install/windows) - Complete Windows installation guide
- [Flutter Installation - macOS](https://docs.flutter.dev/get-started/install/macos) - Complete macOS installation guide
- [Flutter Installation - Linux](https://docs.flutter.dev/get-started/install/linux) - Complete Linux installation guide

### IDE Setup
- [VS Code Flutter Setup](https://docs.flutter.dev/get-started/editor?tab=vscode) - Official VS Code configuration guide
- [Flutter DevTools](https://docs.flutter.dev/development/tools/devtools/overview) - Suite of debugging and profiling tools
- [VS Code Extensions Marketplace](https://marketplace.visualstudio.com/vscode) - Browse and install VS Code extensions

### Flutter Doctor and Troubleshooting
- [Flutter Doctor Documentation](https://docs.flutter.dev/reference/flutter-cli#flutter-doctor) - Official Flutter Doctor reference
- [Troubleshooting Flutter](https://docs.flutter.dev/resources/faq#troubleshooting) - Common issues and solutions
- [Android Setup Guide](https://docs.flutter.dev/get-started/install/windows#android-setup) - Detailed Android toolchain setup

### Environment Configuration
- [Setting up an Editor](https://docs.flutter.dev/get-started/editor) - Complete editor setup guide
- [Configure Flutter SDK Path](https://docs.flutter.dev/get-started/install/windows#update-your-path) - PATH configuration instructions
- [Platform Setup](https://docs.flutter.dev/get-started/install) - Platform-specific setup requirements

### Development Tools
- [Flutter CLI Reference](https://docs.flutter.dev/reference/flutter-cli) - Complete command-line interface documentation
- [Device Testing](https://docs.flutter.dev/get-started/install/windows#set-up-your-android-device) - Set up physical and virtual devices
- [Web Setup](https://docs.flutter.dev/get-started/web) - Configure Flutter for web development
