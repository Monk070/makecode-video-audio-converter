# MakeCode Arcade Video + Audio Converter (and Images!)

A browser-based tool that converts video and audio files into MakeCode Arcade format, with 16-color palette quantization and polyphonic audio synthesis using spectrogram analysis.

![MakeCode Arcade](https://img.shields.io/badge/MakeCode-Arcade-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![No Dependencies](https://img.shields.io/badge/dependencies-none-success)

## 😎 V1.3 Update - 28/12/2025

Now handles images too. Cool!
+ Ostromoukhov dithering in the video converter. 

## ✨ Features

### 🎬 Video Conversion
- **16-color palette quantization** using perceptually-weighted color distance
- **Three dithering modes:** None, Floyd-Steinberg, Ordered (Bayer 4×4)
- **Three scaling modes:** Fit (letterbox), Fill (crop), Stretch
- Configurable output dimensions with 4:3 aspect ratio lock
- Adjustable frame rate and max frame count
- Real-time thumbnail preview grid

### 🎵 Audio Conversion
- **Polyphonic audio reconstruction** using 20 frequency bands
- FFT-based spectrogram analysis with visual output
- Outputs using MakeCode's `queuePlayInstructions()` API
- Configurable period (FFT window) and gain settings
- Web Worker processing keeps UI responsive
- **Direct audio file support** (MP3, WAV, OGG, M4A)
- **Convert audio from video** without processing frames
- **Export video audio as WAV** file

### ⏱️ Time Range Selection
- Select specific portions of longer videos/audio
- Visual timeline with drag handles
- Set start time from current playback position
- Preview selected range before converting

### 📤 Output
- Ready-to-paste TypeScript/JavaScript code
- Three output tabs: Combined, Video-only, Audio-only
- Copy to clipboard or download as `.ts` file
- Code size estimation

## 🚀 Quick Start

1. Open `makecode-arcade-converter.html` in Chrome
2. Drag & drop your video or audio file
3. Adjust settings and time range
4. Click **Convert**
5. Copy the generated code to MakeCode Arcade

## 📖 Usage Guide

### Video + Audio Conversion
1. **Drop a video file** onto the upload area
2. **Adjust video settings** — dimensions, FPS, dithering, scale mode
3. **Configure audio** — period and gain
4. **Select time range** if needed (recommended: under 10 seconds)
5. **Click Convert** and wait for processing
6. **Copy code** from the Combined tab

### Audio-Only from Video
Want just the audio from a video? Two options:

| Button | Result |
|--------|--------|
| **💾 Export as WAV** | Downloads audio as a WAV file |
| **🎵 Convert Audio Only** | Generates MakeCode audio code directly |

### Standalone Audio Files
1. **Drop an audio file** (MP3, WAV, OGG, M4A)
2. Video controls hide automatically
3. **Adjust audio settings** and time range
4. **Click Convert**
5. **Copy code** from the Audio tab

## 🎮 Adding to MakeCode Arcade

### Video Playback
```typescript
// Paste the generated frames array
const frames: Image[] = [ /* generated frames */ ];

// Simple playback loop
let frameIndex = 0;
game.onUpdateInterval(100, function() {
    scene.setBackgroundImage(frames[frameIndex]);
    frameIndex = (frameIndex + 1) % frames.length;
});
```

### Audio Playback
Add this shim to your project (required for `queuePlayInstructions`):

```typescript
namespace music {
    //% shim=music::queuePlayInstructions
    export function queuePlayInstructions(timeDelta: number, buf: Buffer): void {
        // Handled by MakeCode runtime
    }
}
```

Then call the generated `playAudio()` function.

## 🎨 Color Palette

MakeCode Arcade's 16-color palette:

| Char | Color | Hex |
|:----:|-------|-----|
| 0 | Transparent | — |
| 1 | White | `#FFFFFF` |
| 2 | Red | `#FF2121` |
| 3 | Pink | `#FF93C4` |
| 4 | Orange | `#FF8135` |
| 5 | Yellow | `#FFF609` |
| 6 | Teal | `#249CA3` |
| 7 | Green | `#78DC52` |
| 8 | Blue | `#003FAD` |
| 9 | Light Blue | `#87F2FF` |
| a | Purple | `#8E2EC4` |
| b | Light Purple | `#A4839F` |
| c | Dark Purple | `#5C406C` |
| d | Tan | `#E5CDC4` |
| e | Brown | `#91463D` |
| f | Black | `#000000` |

## 🔧 Technical Details

### Audio Processing
- **20 frequency bands** from 50 Hz to 10,240 Hz
- **Cooley-Tukey radix-2 FFT** with Hann windowing
- **Web Worker** for non-blocking processing
- Generates 12-byte sound instructions per note

### Supported Formats

| Video | Audio |
|-------|-------|
| MP4 (H.264) | MP3 |
| WebM (VP8/VP9) | WAV |
| MOV | OGG |
| | M4A |

## 💡 Tips for Best Results

- **Keep clips under 10 seconds** — better for processing time and MakeCode memory
- **Floyd-Steinberg dithering** works best for most video content
- **Fill mode** is ideal for 16:9 YouTube videos
- **Increase gain** (4x–6x) if audio sounds quiet in MakeCode
- **Lower period** (20ms) for fast-changing audio, **raise it** (30ms) for smoother sounds
- **Check browser console** (F12) for detailed progress logs

## ⚠️ Limitations

- Large videos require significant memory during processing
- Audio quality is limited by MakeCode's synthesis capabilities
- Very fast visual changes may not reproduce well at low frame rates
- Some browsers may have codec compatibility issues

## 🌐 Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome | ✅ Recommended |
| Firefox | ✅ Supported |
| Edge | ✅ Supported |
| Safari | ⚠️ May have Web Worker issues |

## 🙏 Credits

- Audio spectrogram approach inspired by [HomeAssistantTycoon's Audio-to-MakeCode-Arcade](https://github.com/HomeAssistantTycoon/Audio-to-MakeCode-Arcade)
- Built with vanilla HTML, CSS, and JavaScript — **zero dependencies**

## 📄 License

MIT License — free to use, modify, and distribute.

---

Made with ❤️ for the MakeCode Arcade community
