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

* Text History & Sync
  All extracted texts are saved to user account
  Accessible across devices via web dashboard
  Search and filter extracted texts by date, URL, or content

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

### Backend (Java Spring Boot)

* REST API endpoints for text extraction, history, and user management
* JWT-based authentication and authorization
* Cloud vision API integration (Google Vision or Azure Computer Vision)
* Rate limiting and usage tracking
* User account and subscription management
* Text history persistence in PostgreSQL

---

## Tech Stack

### Chrome Extension Frontend
* TypeScript / JavaScript
* Chrome Extensions API (Manifest V3)
* Tesseract.js for local OCR (WebAssembly)
* Chrome Storage API for local history backup

### Backend (Java)
* Spring Boot 3.x
* Spring Web (REST API)
* Spring Data JPA / Hibernate (ORM)
* Spring Security + JWT (authentication)
* PostgreSQL (database)
* Google Cloud Vision API or Azure Computer Vision (advanced OCR)

### Database
* PostgreSQL 14+
* JPA / Hibernate for ORM mapping

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

## Database Schema

### Users Table
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    subscription_tier VARCHAR(50) DEFAULT 'FREE',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

### Extracted Texts Table
```sql
CREATE TABLE extracted_texts (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    original_text TEXT NOT NULL,
    page_url VARCHAR(500),
    image_source VARCHAR(500),
    extraction_mode VARCHAR(50) NOT NULL, -- 'LOCAL' or 'ADVANCED'
    confidence_score FLOAT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),

    INDEX idx_user_id (user_id),
    INDEX idx_created_at (created_at)
);
```

### Usage Statistics Table
```sql
CREATE TABLE usage_stats (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    extractions_count INT DEFAULT 0,
    local_mode_count INT DEFAULT 0,
    advanced_mode_count INT DEFAULT 0,
    month DATE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),

    UNIQUE (user_id, month),
    INDEX idx_user_month (user_id, month)
);
```

### API Keys Table (for advanced mode quota tracking)
```sql
CREATE TABLE api_keys (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    key_hash VARCHAR(255) UNIQUE NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW(),
    expires_at TIMESTAMP,

    INDEX idx_user_id (user_id)
);
```

---

## Backend Setup (Java Spring Boot)

### Prerequisites
* Java 17 or higher
* Spring Boot 3.x
* PostgreSQL 14+
* Maven 3.8+

### Database Migration
Create the PostgreSQL database:
```bash
createdb snatchy_db
```

Run Spring Boot with migration enabled:
```bash
mvn spring-boot:run
```

Spring Boot will automatically run Flyway or Liquibase migrations to create tables.

### REST API Endpoints

**Authentication:**
- `POST /api/auth/register` - Create new user account
- `POST /api/auth/login` - Authenticate and receive JWT token

**Text History:**
- `POST /api/texts` - Save extracted text (requires JWT)
- `GET /api/texts` - Retrieve user's text history with pagination (requires JWT)
- `GET /api/texts/{id}` - Get specific text entry (requires JWT)
- `DELETE /api/texts/{id}` - Delete text entry (requires JWT)
- `GET /api/texts/search?query=...` - Search text history (requires JWT)

**Advanced OCR:**
- `POST /api/ocr/advanced` - Extract text using cloud vision API (requires JWT, Pro tier)

**User Profile:**
- `GET /api/users/profile` - Get user details (requires JWT)
- `PUT /api/users/profile` - Update user profile (requires JWT)

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
