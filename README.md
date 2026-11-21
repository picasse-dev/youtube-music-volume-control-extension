# 🎵 YouTube Music Volume Control

**Control YouTube Music's volume from anywhere in your browser — without leaving fullscreen, switching tabs, or interrupting your experience.**

[![Manifest V3](https://img.shields.io/badge/Manifest-V3-blue)](https://developer.chrome.com/docs/extensions/develop/migrate/what-is-mv3)
[![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Chrome](https://img.shields.io/badge/Chrome-Compatible-brightgreen)](https://www.google.com/chrome/)
[![Firefox](https://img.shields.io/badge/Firefox-Compatible-orange)](https://www.mozilla.org/firefox/)

---

## 💡 The idea behind the project

Have you ever been watching a YouTube video in fullscreen while listening to music on YouTube Music in another tab, only to realize that the music is too loud or too quiet?

Normally, you would have to leave fullscreen, switch tabs, open YouTube Music, adjust the volume, and return to what you were doing.

A small inconvenience, but one that gets annoying when it happens repeatedly.

I wanted a simpler solution.

**YouTube Music Volume Control** was created to solve exactly that problem.

With just two keyboard shortcuts, you can increase or decrease YouTube Music's volume from any active browser tab, even while watching a fullscreen video.

No tab switching. No interruptions. Just press a shortcut and keep enjoying your content.

## ✨ Features

- **Global keyboard shortcuts:** Control YouTube Music from your currently active browser tab.
- **Fullscreen support:** Adjust the music volume without exiting fullscreen or interrupting your video.
- **Independent volume control:** Change YouTube Music's player volume without modifying your system's master volume.
- **5% volume increments:** Fine-grained volume adjustments.
- **Volume boundaries:** The volume is always kept between 0% and 100%.
- **Visual notifications:** Instantly see the current volume level through a small on-screen notification.
- **Lightweight:** Built with vanilla JavaScript and native browser extension APIs.
- **Cross-browser support:** Designed for Chromium-based browsers and Firefox.

## ⌨️ Keyboard Shortcuts

| Action | Shortcut | Behavior |
|---|---|---|
| Increase volume | `Ctrl + Shift + ↑` | Increases volume by 5% |
| Decrease volume | `Ctrl + Shift + ↓` | Decreases volume by 5% |

### Example

If YouTube Music is currently playing at 40% volume:

- Press `Ctrl + Shift + ↑` → Volume becomes 45%.
- Press `Ctrl + Shift + ↑` again → Volume becomes 50%.
- Press `Ctrl + Shift + ↓` → Volume returns to 45%.

The volume cannot exceed 100% or go below 0%.

## 🖥️ How it works

The extension uses Chrome Extension APIs and JavaScript to communicate between different parts of the browser.

It consists of three main components:

### 1. Background Service Worker (`background.js`)

Acts as the central coordinator of the extension.

Its responsibilities include:

- Listening for keyboard shortcut commands.
- Finding open YouTube Music tabs.
- Sending volume control messages to YouTube Music.
- Requesting the updated volume level.
- Sending notification messages to the currently active tab.

### 2. Content Script (`content.js`)

Runs directly inside YouTube Music.

Its responsibilities include:

- Finding the HTML `<video>` element used by the player.
- Increasing or decreasing its volume.
- Keeping the volume within the allowed range.
- Returning the current volume level.
- Displaying volume notifications inside YouTube Music.

### 3. Notification Script (`notification.js`)

Runs on supported web pages and handles global volume notifications.

Its responsibilities include:

- Listening for notification messages.
- Creating a visual notification dynamically.
- Displaying the current volume percentage.
- Automatically removing the notification after a short delay.

### 4. Manifest (`manifest.json`)

Defines the extension's configuration, permissions, background execution, content scripts, supported websites, and keyboard shortcuts.

## 🏗️ Project Structure

```text
youtube-music-volume-control/
│
├── manifest.json       # Extension configuration
├── background.js       # Background logic and communication
├── content.js          # YouTube Music volume control
├── notification.js     # Global volume notifications
│
└── README.md            # Project documentation
```

## 🌐 Browser Compatibility

The extension is designed to work across multiple browsers.

| Browser | Compatibility |
|---|---|
| Google Chrome | Supported |
| Mozilla Firefox | Supported |
| Microsoft Edge | Chromium-based compatibility expected |
| Brave | Chromium-based compatibility expected |
| Opera | Chromium-based compatibility expected |
| Vivaldi | Chromium-based compatibility expected |

**Note:** Chrome and Firefox have been tested. Other Chromium-based browsers should be compatible, but have not necessarily been independently tested.

Safari is not currently supported.

## 🚀 Installation

You can install the extension locally without publishing it to a browser extension store.

### Google Chrome and Chromium-based browsers

1. Clone or download this repository:

   ```bash
   git clone https://github.com/YOUR_USERNAME/youtube-music-volume-control.git
   ```

2. Open your browser and navigate to:

   ```text
   chrome://extensions/
   ```

3. Enable **Developer mode**.

4. Click **Load unpacked**.

5. Select the project folder containing `manifest.json`.

6. Open YouTube Music:

   https://music.youtube.com/

7. Start playing music and use the keyboard shortcuts.

The same general procedure applies to compatible Chromium-based browsers, although their extension management pages may differ.

### Mozilla Firefox

1. Clone or download the repository.
2. Open Firefox.
3. Navigate to:

   ```text
   about:debugging#/runtime/this-firefox
   ```

4. Click **Load Temporary Add-on...**.
5. Select the project's `manifest.json`.
6. Open YouTube Music and start playing music.

**Note:** Temporary add-ons are removed when Firefox closes. A permanent installation requires packaging and signing the extension through Mozilla's distribution process.

## ⚙️ Technical Details

| Component | Technology |
|---|---|
| Language | JavaScript |
| Extension Architecture | Manifest V3 |
| Browser APIs | Chrome Extension APIs |
| Communication | Runtime Message Passing |
| Volume Control | HTMLMediaElement API |
| Notifications | Dynamic DOM Manipulation |
| Styling | CSS |

### Volume Management

The extension uses the native HTML media volume property.

To increase volume:

```javascript
video.volume = Math.min(1, video.volume + 0.05);
```

To decrease volume:

```javascript
video.volume = Math.max(0, video.volume - 0.05);
```

The volume property uses a value between `0` and `1`, where:

- `0` represents 0% volume.
- `0.5` represents 50% volume.
- `1` represents 100% volume.

This allows the extension to make precise volume adjustments without interacting with the operating system's audio settings.

## 🔐 Permissions

The extension uses browser permissions to communicate with open tabs and interact with YouTube Music.

- `tabs`: Allows the extension to query browser tabs and send messages to them.
- `activeTab`: Requests access to the active tab when applicable.
- `scripting`: Provides access to the browser scripting API.
- `host_permissions`: Defines the websites where the extension can interact with page content.

The extension needs access to YouTube Music to control its player and access to other supported pages to display volume notifications.

## ⚠️ Limitations

- YouTube Music must be open in at least one browser tab.
- The extension controls the HTML media player's volume, not the operating system's master volume.
- Volume adjustments are applied in increments of 5%.
- If YouTube Music changes its internal player implementation, the extension may require updates.
- Browser keyboard shortcuts may conflict with shortcuts registered by other extensions or the browser itself.
- Other Chromium-based browsers should be tested individually.

## 🛠️ Future Improvements

Some ideas for future development:

- [ ] Add a configurable volume increment.
- [ ] Add customizable keyboard shortcuts.
- [ ] Improve multi-tab player detection.
- [ ] Refactor and unify notification handling.
- [ ] Improve error handling and edge cases.
- [ ] Add a settings interface.
- [ ] Prepare an official browser store release.
- [ ] Explore Safari compatibility.

## 🤝 Contributing

Contributions, suggestions, and bug reports are welcome!

If you find a bug or have an idea that could improve the extension, feel free to open an issue or submit a pull request.

## 📄 License

This project is licensed under the MIT License.

See the [LICENSE](LICENSE) file for details.

---

**Built to solve a small annoyance and make everyday browsing a little more convenient.**

Enjoy your music without interruptions! 🎧