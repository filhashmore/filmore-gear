# FILMORE Gear Inventory

A comprehensive web application for touring production managers and backline techs to track, manage, and update gear inventory.

## Features

### 📊 Dashboard
- Real-time overview of total inventory items
- Maintenance alerts for gear needing attention
- Consumables restocking status
- Pending acquisition approvals
- ASAP acquisition items highlight

### 📦 Inventory Management
- Full gear catalog with status tracking (Good, Maintenance, Unknown)
- Show type classification (DRIVE, FLY, Storage)
- Pack location tracking
- Stock levels and pricing

### ⚡ Consumables
- Tape, batteries, cables, and band supplies
- Status tracking (Full Stock, Needed, Need Soon, Additional)
- Direct purchase links
- Quantity needed tracking

### 🛒 Acquisitions (Approval Workflow)
- Research and add new gear needs
- Rent vs Own classification
- Priority status (Acquire ASAP, Needed, Additional, etc.)
- **Approval workflow**: PM submits → Management/Artist approves or denies
- Role-based views (PM, Management, Artist)

### ✈️ FLY Pack
- Visual baggage assignment by crew member (Joel, Drew, Reid, Tyler)
- Checked bags vs Carry-ons organization
- Quick reference for fly dates

### 🎸 Band Gear
- Individual gear lists for each band member
- Joel's gear (drums, monitors, playback)
- Reid's gear (electric guitars, pedalboard)
- Drew's gear (bass, pedalboard)

## Access

Access code required. Contact admin for credentials.

## Usage

### For Production Managers
1. Add, edit, and update all gear items
2. Flag acquisitions for approval
3. Track maintenance needs
4. Manage consumable stock levels

### For Management/Artist
1. Switch to Management or Artist view in Acquisitions tab
2. Review pending acquisition requests
3. Approve or deny items with one click

## Tech Stack

- React 18 (CDN)
- Tailwind CSS
- Lucide Icons
- LocalStorage for data persistence

## Deployment

Static HTML deployment - works on any web server including:
- Vercel
- Netlify
- GitHub Pages
- Any static hosting

## Data Persistence

All data is stored in browser localStorage. Data persists across sessions but is device-specific.

## Security

- Access code protection
- Security headers configured for Vercel deployment
- No sensitive data stored server-side

---

Built for FILMORE Production by FiL Hash
