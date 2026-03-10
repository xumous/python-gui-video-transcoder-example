# python-gui-video-transcoder-example

A simple example of a video transcoder with a graphical user interface built in Python. Demonstrates basic video format
conversion using FFmpeg, ideal for learning how to integrate GUI frameworks like PyQt with multimedia processing.

# Python GUI Video Transcoder Example (with AVS3 Support)

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![PyQt6](https://img.shields.io/badge/PyQt-6-green.svg)](https://pypi.org/project/PyQt6/)
[![ffmpeg](https://img.shields.io/badge/ffmpeg-8.0.1-red.svg)](https://ffmpeg.org/)

A graphical video transcoding tool based on **PyQt6** and **ffmpeg-python**, specifically designed for efficiently
transcoding **8K Spring Festival Gala AVS3 videos**. It supports GPU acceleration (NVIDIA NVENC), batch task management,
detailed audio/video parameter adjustments, and provides a user-friendly interface.

> Why write this?  
> Just for fun. All functionality can essentially be achieved via command line, but I got tired of typing commands every
> time, so I built a frontend. If you also dislike memorizing ffmpeg parameters, feel free to give this a try.

---

## ✨ Features

* **AVS3 Decoding Support**: Built-in ffmpeg 8.0.1, perfectly decodes 8K Spring Festival Gala AVS3 videos.
* **GPU Acceleration**: Supports NVIDIA NVENC (H.264 / H.265), greatly improving transcoding speed.
* **Batch Task Queue**: Add multiple files simultaneously, set maximum concurrent tasks, and automatically queue
  transcoding.
* **Detailed Parameter Adjustments**:
    * Video: Encoding format (H.264/H.265/VP9/AV1/Copy), resolution, bitrate, CRF, frame rate, keyframe interval,
      rotation/flip, etc.
    * Audio: Encoding format (AAC/MP3/AC3/FLAC/Opus/Copy), sample rate, bitrate, channels, volume adjustment.
* **Intelligent Output Paths**: Automatically generate unique filenames to avoid overwriting; supports output to source
  directory or custom folder.
* **Real-Time Progress Display**: Task list shows transcoding progress, output file size, and percentage change in size.
* **Log Window**: Records operations and error messages for easy troubleshooting.
* **Drag-and-Drop File Addition**: Simply drag video files into the window to add tasks.
* **Global Settings**: Configurable options such as default GPU acceleration, number of concurrent tasks, etc.
* **Cross-Platform** (mainly Windows): Code is compatible with Linux/macOS, but you need to provide your own ffmpeg.

---

## 🚀 Quick Start

### 1. Install Python Environment

Requires Python 3.8 or higher.

```bash
# Clone the repository
git clone https://github.com/xumous/python-gui-video-transcoder-example.git
cd python-gui-video-transcoder-example

# Install dependencies
pip install PyQt6 ffmpeg-python
```

### 2. Obtain ffmpeg

This tool depends on the ffmpeg executable. You can:

* **Manually download** ffmpeg (8.0.1 or newer) and place `ffmpeg.exe`, `ffprobe.exe` in the same directory as the
  script, or add them to the system PATH.
* **Use a packaged version**: If you plan to build a standalone EXE, place the ffmpeg binaries in the path specified by
  the packaging command (see below).

### 3. Run

```bash
python "python-gui-video-transcoder-example.py"
```

---

## 📖 User Guide

### Adding Tasks

* Click the **+ New Task** button or simply drag video files into the window.
* In the pop-up dialog, set the output format, video/audio parameters.
* Click **OK** to add the task to the queue, or **OK and Start** to begin immediately.

### Task Management

* **Status Column**: Displays task status (Waiting/Transcoding/Completed/Error) and a progress bar.
* **Action Buttons**: Each task has a "Delete" button on the right.
* **Batch Operations**: The toolbar provides buttons for "Remove Selected", "Clear List", "Stop All", "Start All".

### Global Settings

Via the menu **Settings → Global Settings**, you can adjust:

* Default GPU acceleration
* Auto-detection of video parameters
* Maximum number of concurrent tasks

### Output Files

* Output files are saved in the source file directory by default, with filenames
  like `original_filename_timestamp.extension`.
* For completed tasks, you can click the filename link in the list to directly open the output file.

---

## 🛠 Building a Standalone Executable

This tool supports packaging into a single EXE file using **PyInstaller** or **Nuitka**, bundling the ffmpeg binaries.

### Prepare ffmpeg Binaries

Download the Windows version from the [ffmpeg official website](https://ffmpeg.org/download.html) (recommended 8.0.1 or
higher), and place the following three files in a suitable location (e.g., `C:\\Java2025\\`):

* `ffmpeg.exe`
* `ffprobe.exe`
* `ffplay.exe` (optional)

### Packaging with PyInstaller

```powershell
python -m PyInstaller --onefile --windowed `
  --add-binary "C:\\Java2025\\ffmpeg.exe;." `
  --add-binary "C:\\Java2025\\ffprobe.exe;." `
  --add-binary "C:\\Java2025\\ffplay.exe;." `
  --hidden-import ffmpeg `
  --name "VideoTranscoder" `
  "python-gui-video-transcoder-example.py"
```

### Packaging with Nuitka

```powershell
python -m nuitka --onefile --windows-disable-console `
  --include-data-file="C:\\Java2025\\ffmpeg.exe=ffmpeg.exe" `
  --include-data-file="C:\\Java2025\\ffprobe.exe=ffprobe.exe" `
  --include-data-file="C:\\Java2025\\ffplay.exe=ffplay.exe" `
  --include-module=PyQt6.sip `
  --enable-plugin=pyqt6 `
  --output-dir=dist `
  --product-name="Video Transcoder" `
  --file-version="1.4.0" `
  "python-gui-video-transcoder-example.py"
```

The packaged EXE file will be located in the `dist` directory and can be run directly without requiring Python or ffmpeg
to be installed.

---

## 📦 Dependencies

* Python ≥ 3.8
* [PyQt6](https://pypi.org/project/PyQt6/)
* [ffmpeg-python](https://pypi.org/project/ffmpeg-python/)
* ffmpeg 8.0.1 (must be downloaded or packaged separately)

---

## ❓ FAQ

### Q: Why isn't GPU acceleration working?

A: Ensure your graphics card is NVIDIA and you have the latest drivers installed. GPU acceleration only supports H.264
and H.265 encoding (using `h264_nvenc` / `hevc_nvenc`). If it still fails, check if your ffmpeg supports NVENC (verify
with `ffmpeg -encoders | findstr nvenc`).

### Q: What should I do if an error occurs during transcoding?

A: Check the log window below; the error message usually indicates the specific cause. Common issues include: corrupted
input file, no write permission for output path, ffmpeg missing the required encoder (e.g., AV1 requires libaom-av1 to
be installed separately).

### Q: How do I add custom resolutions or bitrates?

A: Simply type the custom value directly into the corresponding dropdown box (e.g., `1920x800` or `5000`); manual
editing is supported.

### Q: Is macOS/Linux supported?

A: The code itself is cross-platform, but you need to replace ffmpeg with the executable for the respective platform.
The "System" option in global settings is for display only and does not affect functionality.

---

## 🙏 Acknowledgements

* [ffmpeg](https://ffmpeg.org/): Powerful multimedia processing tool.
* [PyQt6](https://www.riverbankcomputing.com/software/pyqt/): Excellent Python GUI framework.
* [ffmpeg-python](https://github.com/kkroening/ffmpeg-python): Pythonic ffmpeg wrapper.

---

## 📄 License

The code of this project is licensed under the [MIT License](LICENSE).  
ffmpeg binaries are subject to their own [LGPL/GPL licenses](https://ffmpeg.org/legal.html).

---

> Note: This tool is just a personal boredom project and may contain unknown bugs. If you encounter issues, feel free to
> submit an Issue, but due to laziness, it might not be fixed promptly.