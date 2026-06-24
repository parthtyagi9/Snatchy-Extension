# Snatchy Extension

Snatchy is a privacy-first Chrome Extension that extracts selectable text from images and non-selectable web content using local OCR.

The public extension repo is the product-facing half of a full-stack portfolio project. It demonstrates Chrome Manifest V3, TypeScript, content scripts, service workers, DOM overlays, browser storage, Tesseract.js OCR, and an optional Spring Boot backend integration.

## Demo

Add a short GIF or screenshots here after building the first demo:

- Image hover state with the Snatchy action
- Local OCR loading state
- Selectable text overlay
- Popup settings for advanced backend mode

## Engineering Highlights

- **Chrome Extension / Manifest V3:** content script injection, service worker lifecycle, popup UI, and scoped browser permissions.
- **Local-first OCR:** Tesseract.js runs in the browser so the default extraction path does not send user images to a server.
- **Selectable DOM overlay:** extracted text is rendered as normal selectable text instead of a dead screenshot or copied-only modal.
- **Advanced mode hook:** optional backend URL and JWT settings connect to a private Spring Boot API for provider-backed OCR.
- **Testable TypeScript:** settings normalization is isolated and covered with Vitest.

## Architecture

```text
Web page image
    |
    v
Content script hover detector
    |
    +--> Local OCR with Tesseract.js
    |
    +--> Optional /api/ocr/advanced backend call
    |
    v
Selectable DOM overlay

<<<<<<< HEAD
* **Hover-based interaction** - A small button appears when you hover over an image
* **Selectable text overlay** - Text is rendered as selectable regions on top of images
* **Standard copy behavior** - Use normal keyboard shortcuts to copy extracted text
* **Local OCR (Free)** - Runs entirely in the browser using Tesseract.js, no data sent externally
* **Advanced Mode (Optional)** - Connect to a backend for higher accuracy on complex images
* **Privacy-first** - Local mode keeps all data on your device

---

## Modes

### Local Mode (Default, Always Free)

* Uses in-browser OCR (Tesseract.js)
* Fast and instananeous
* Works well for clear images and screenshots
* No backend connection required
* **Your data never leaves your device**

### Advanced Mode (Optional, Requires Backend)

* Connects to a private backend server for enhanced OCR
* Higher accuracy for:
  * Blurry or low-resolution images
  * Complex layouts and stylized fonts
  * Scanned documents and handwriting
* Requires self-hosted or private backend server
* Requires authentication token

---

## Installation & Usage

### For Users

1. Install from Chrome Web Store (coming soon)
2. Click the extension icon to open settings
3. Hover over any image on a webpage
4. Click "Extract Text" button
5. Select and copy the text

### For Developers

1. Clone the repository:
   ```bash
   git clone https://github.com/parthtyagi9/Snatchy-Extension.git
   cd Snatchy-Extension
   ```

2. Open Chrome and navigate to `chrome://extensions/`

3. Enable **Developer Mode** (top right)

4. Click **Load unpacked** and select this directory

---

## Development

### Tech Stack
* **Language:** TypeScript / JavaScript
* **API:** Chrome Extensions Manifest V3
* **OCR:** Tesseract.js (WebAssembly)
* **Storage:** Chrome Storage API (for local backups)

### Architecture

```
┌─────────────────────────────┐
│  Content Scripts (DOM)      │  Extract images, render overlays
├─────────────────────────────┤
│  Service Worker             │  Handle background logic
├─────────────────────────────┤
│  Tesseract.js (Local OCR)   │  Process images in-browser
├─────────────────────────────┤
│  Optional Backend API       │  Advanced OCR (if configured)
└─────────────────────────────┘
=======
Popup settings -> Chrome Storage Sync -> Content script runtime config
>>>>>>> 2747555 (changes made pushing)
```

## Tech Stack

- TypeScript
- Chrome Extensions Manifest V3
- Tesseract.js / WebAssembly OCR
- Chrome Storage API
- Vite
- Vitest

## Local Development

```bash
npm install
npm test
npm run build
```

Then load the built extension:

1. Open `chrome://extensions/`
2. Enable Developer Mode
3. Click **Load unpacked**
4. Select the `dist` directory

## Advanced Backend Mode

The extension defaults to local OCR. Advanced mode can be enabled from the popup with:

- `backendUrl`, default `http://localhost:8080`
- `advancedModeEnabled`, default `false`
- `authToken`, JWT from the private backend
- `localOcrLanguage`, default `eng`

Advanced mode calls:

- `POST /api/ocr/advanced`

The backend repository is private and contains the Spring Boot API for JWT auth, OCR history, usage tracking, provider abstraction, Dockerized PostgreSQL, and integration tests.

## Resume Bullets

- Built a privacy-first Chrome Extension using Manifest V3, content scripts, and Tesseract.js to extract selectable text from images and non-selectable web content.
- Implemented a TypeScript OCR workflow with hover detection, browser storage settings, local OCR, advanced backend configuration, and selectable DOM overlays.
- Designed the extension as the public frontend for a full-stack OCR system backed by a private Spring Boot, PostgreSQL, JWT, and Docker architecture.
