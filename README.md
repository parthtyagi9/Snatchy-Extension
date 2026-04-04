# Snatchy

Take text from anywhere on the web, even where it is not selectable.

Snatchy is a Chrome extension that allows users to select and copy text from images, scanned documents, and other non-selectable content using OCR and AI.

---

## Overview

Many websites contain text inside images, screenshots, or rendered canvases where normal copy and paste does not work. Snatchy solves this by detecting text inside images and overlaying selectable regions directly on the page.

The extension is designed with a local-first approach for performance and privacy, with an optional AI-enhanced mode for improved accuracy.

---

## Features

* Hover-based interaction model
  When hovering over an image, a small action button appears to extract text

* Selectable text overlay
  Detected text is rendered as selectable regions on top of the image

* Standard copy behavior
  Users can drag and copy text using normal keyboard shortcuts

* Local OCR (Basic Mode)
  Runs entirely in the browser using Tesseract.js
  No API calls required

* AI Enhanced Mode (Advanced)
  Uses a backend with a stronger vision model for higher accuracy
  Works better on low-quality, complex, or stylized text

* Privacy-first design
  Local mode does not send data externally

---

## Modes

### Basic Mode (Default)

* Uses in-browser OCR (Tesseract.js)
* Fast and free
* Works well for clear images and screenshots
* No data leaves the user’s device

### Advanced AI Mode (Optional)

* Uses a backend OCR / vision API
* Higher accuracy for:

  * Blurry images
  * Complex layouts
  * Stylized fonts
  * Scanned documents
* Requires API usage and may be part of a paid plan

---

## How It Works

1. User hovers over an image
2. A small "Extract Text" button appears
3. User clicks the button
4. The extension captures the image or region
5. OCR processes the image and detects text with bounding boxes
6. The extension renders selectable overlays
7. User selects and copies text normally

---

## Architecture

### Chrome Extension

* Manifest V3
* Content scripts for DOM interaction and overlays
* Service worker for background logic
* Optional popup or side panel for settings

### OCR Layer

* Basic Mode: Tesseract.js (WebAssembly, runs in-browser)
* Advanced Mode: Backend API with cloud OCR / vision model

### Backend (Optional)

* API endpoint for AI-enhanced OCR
* Authentication and rate limiting
* Usage tracking
* Subscription and billing logic

---

## Tech Stack

* JavaScript or TypeScript
* Chrome Extensions API (Manifest V3)
* Tesseract.js for local OCR
* Node.js and Express (backend, optional)
* Cloud OCR / Vision APIs for advanced mode

---

## Installation (Development)

1. Clone the repository

   git clone https://github.com/parthtyagi9/snatchy.git
   cd snatchy

2. Open Chrome and go to chrome://extensions/snatchy (Work in prog)

3. Enable Developer Mode

4. Click "Load unpacked"

5. Select the project directory

---

## Usage

* Hover over any image on a webpage
* Click "Extract Text"
* Wait for processing to complete
* Select text directly from the overlay
* Copy using standard shortcuts

---

## Pricing Model (Planned)

Free Tier:

* Local OCR
* Basic extraction features

Pro Tier:

* AI-enhanced OCR
* Better accuracy on difficult inputs
* Batch processing and additional tools

---

## Limitations

* Handwriting recognition is limited
* Very low-resolution images may reduce accuracy
* Complex layouts may require advanced mode
* Some protected or cross-origin content may not be accessible

---

## Roadmap

* Region-based selection (drag to scan specific areas)
* Translation of extracted text
* Summarization of extracted content
* Batch OCR support
* Save and manage extracted snippets
* Improved layout detection

---

## Contributing

Contributions are welcome. Please open an issue before making major changes to discuss the approach.

---

## License

MIT License

---

## Rationale

This project demonstrates:

* Chrome extension development using Manifest V3
* Client-side OCR using WebAssembly
* DOM manipulation and overlay rendering
* Scalable architecture with optional backend integration
* Thoughtful tradeoffs between performance, privacy, and accuracy
