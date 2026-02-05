# WASMTainer
## WASM App Installer

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

A Python-based installer script that converts WASM (WebAssembly) distribution files into a standalone desktop application. It bundles the WASM app with a local HTTP server, PyQt6-based browser window, and uses PyInstaller to create a single executable binary.

This tool is ideal for developers who want to distribute WASM-based web apps (e.g., built with Emscripten or similar) as native-like desktop apps on Linux (with plans for cross-platform expansion).

## Features

- **Automated Installation**: Copies WASM files, icons, and creates necessary directories.
- **Launcher Script**: Generates a Python launcher using PyQt6 to run the app in a dedicated window with a local server.
- **Configuration Management**: Handles app config, persistent storage, and port management.
- **Desktop Integration**: Creates a `.desktop` entry for Linux for easy launching from menus.
- **Standalone Binary**: Builds a single executable using PyInstaller for easy distribution.
- **Cross-Origin Policies**: Supports COOP/COEP headers for secure WASM execution.
- **Persistent Data**: Uses Qt's persistent storage for browser data (e.g., localStorage, cache).

## Requirements

- **Python 3.8+** (tested on 3.12)
- **PyQt6**: For the GUI browser window.
- **PyInstaller**: For building the standalone binary.
- **Operating System**: Currently optimized for Linux. macOS/Windows support can be added with minor modifications.
- **WASM Distribution Files**: Your WASM app should include files like `index.html`, `.wasm`, `.js`, etc., in the current directory.

Install dependencies via pip:

```bash
pip install PyQt6 pyinstaller
```

## Usage

1. **Prepare Your WASM App**:
   - Place all WASM distribution files (e.g., `index.html`, `app.wasm`, `app.js`) in a directory.
   - Ensure you have an icon file (e.g., `icon.png`, `.ico`, or `.icns`).

2. **Run the Installer**:
   - Navigate to the directory containing your WASM files and the installer script (`wasminstaller.py`).
   - Execute the script with required arguments.

   ```bash
   python3 wasminstaller.py <app_name> <icon_path> [options]
   ```

### Arguments

- `app_name` (required): Lowercase name for the app (alphanumeric, hyphens/underscores allowed, e.g., `my-wasm-app`).
- `icon` (required): Path to the app icon file (e.g., `icon.png`).
- `--display-name` (optional): Human-readable display name (defaults to title-cased `app_name`).
- `--install-dir` (optional): Custom installation directory (defaults to `~/UserApps/<app_name>`).
- `--description` (optional): Short app description.
- `--author` (optional): Author name.
- `--version` (optional): App version (defaults to `1.0.0`).

### Example

```bash
python3 wasminstaller.py my-wasm-app icon.png --display-name "My WASM App" --description "A cool WASM-based desktop app" --version "1.0.0"
```

This will:
- Install to `~/UserApps/my-wasm-app`.
- Create a launcher script, config, and desktop entry.
- Build a standalone binary in `~/UserApps/my-wasm-app/dist/my-wasm-app`.

After installation, launch the app via the desktop entry or directly:

```bash
~/UserApps/my-wasm-app/dist/my-wasm-app
```

## Installation Process Overview

The script performs the following steps:

1. Creates directory structure in the install dir.
2. Copies WASM files (skipping hidden files and the installer itself).
3. Copies the icon.
4. Generates a `launcher.py` script.
5. Creates a `config.json` for server settings.
6. (Linux only) Creates a `.desktop` file in `~/.local/share/applications`.
7. Builds a standalone binary using PyInstaller.

## Development and Debugging

- **DevTools**: Set the environment variable `WASM_DEVTOOLS=true` to enable QtWebEngine remote debugging on port 9223.
- **Custom Ports**: Edit `config.json` in the install dir to change server port or other settings.
- **Logs**: The launcher suppresses server logs by default; modify `CORSHTTPRequestHandler` for verbose output.

## Troubleshooting

- **Port Conflicts**: The launcher automatically finds an available port starting from 8080.
- **PyInstaller Issues**: Ensure PyInstaller is installed and compatible with your Python version. If build fails, check the console output.
- **Missing Dependencies**: If PyQt6 isn't found, install it via pip.
- **Icon Not Showing**: Ensure the icon file is in a supported format and exists.
- **WASM Not Loading**: Verify COOP/COEP headers are required for your WASM app; the server includes them.

## Contributing

Contributions are welcome! Fork the repo, make changes, and submit a pull request. For major changes, open an issue first.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built with [PyQt6](https://www.riverbankcomputing.com/software/pyqt/) for the GUI.
- Uses [PyInstaller](https://pyinstaller.org/) for bundling.
