# Chandoba Comics Digital Archive

## Project Overview

This is a **single-page web application** that serves as a digital archive for Chandoba Comics, a classic Marathi children's literature magazine published from 1960 to 2006. The application provides a modern, searchable interface to browse and read over 500 vintage comic issues hosted on Archive.org.

**Live Data Source:** Internet Archive (archive.org) via their Advanced Search API  
**Target Audience:** Marathi heritage preservation enthusiasts and vintage comic collectors

---

## Technology Stack

| Component | Technology |
|-----------|------------|
| **Frontend** | Vanilla HTML5, CSS3, JavaScript (ES6+) |
| **Styling** | Custom CSS with CSS Variables, Google Fonts (Inter, Playfair Display) |
| **Data Source** | Internet Archive Advanced Search API |
| **Storage** | Browser LocalStorage (for bookmarks and UI state) |
| **No Build Step** | Pure static HTML file - no compilation required |

---

## Project Structure

```
Chandoba/
└── index.html          # Complete single-file application (~1500 lines)
```

The entire application is contained in a single HTML file:

- **Lines 1-8**: HTML boilerplate and meta tags
- **Lines 8-898**: Embedded CSS styles (responsive design, animations, theming)
- **Lines 900-1034**: HTML structure (header, controls, content areas, modal)
- **Lines 1036-1523**: JavaScript application logic

---

## Architecture

### Data Flow
1. On page load, the app attempts to fetch comic metadata from Archive.org API
2. If the API fails/times out (10s timeout), it falls back to embedded data generation
3. Comics are organized by year (1960-2006) and displayed in collapsible sections
4. User interactions (search, filter, bookmarks) are handled client-side

### Key Data Structures

```javascript
// Comic object format
{
    identifier: "Chandoba_Marathi_YYYY_MM",
    title: "Chandoba Marathi YYYY - MonthName",
    year: number,
    month: number (1-12),
    monthName: string,
    url: "https://archive.org/details/{identifier}",
    embedUrl: "https://archive.org/stream/{identifier}?ui=embed",
    thumbnail: "https://archive.org/services/img/{identifier}"
}

// LocalStorage keys
- 'chandobaBookmarks': string[] of comic identifiers
- 'chandobaCollapsedYears': number[] of collapsed year sections
```

### External Dependencies
- **Google Fonts**: Inter (UI), Playfair Display (logo)
- **Archive.org**: API for metadata, embedded reader, thumbnails

---

## Features

### Core Functionality
- **Browse by Year**: 46 years of comics (1960-2006, with some years missing)
- **Search**: Real-time search by title, month, or year (300ms debounce)
- **Filter**: Filter by specific year
- **Sort**: Newest/Oldest first, Title A-Z/Z-A
- **Two View Modes**: Grid view (thumbnails) and List view (compact)

### User Personalization
- **Bookmarks**: Save favorite comics to localStorage
- **Bookmark Import/Export**: JSON file backup/restore
- **Persistent UI State**: Collapsed year sections remembered across sessions

### Reader Experience
- **Inline Reader**: Modal iframe with Archive.org embedded viewer
- **External Link**: Direct link to Archive.org page
- **Keyboard Navigation**: ESC key closes reader modal

---

## Development Guidelines

### Code Style
- **CSS**: Uses CSS custom properties (variables) for theming
- **JavaScript**: ES6+ features (arrow functions, const/let, template literals, async/await)
- **HTML**: Semantic markup with accessibility considerations

### No Build Process
This is a **static site** - simply open `index.html` in a browser or serve with any static file server:

```bash
# Python 3
python -m http.server 8000

# Node.js (npx serve)
npx serve .

# PHP
php -S localhost:8000
```

### Browser Compatibility
- Modern browsers supporting ES6, CSS Grid, and Fetch API
- LocalStorage required for bookmark functionality
- Responsive design supports mobile devices

---

## Testing

### Manual Testing Checklist
1. **API Load**: Verify comics load from Archive.org on fresh load
2. **Offline Mode**: Disconnect internet, verify embedded fallback works
3. **Search**: Test search by title, year, and partial matches
4. **Bookmarks**: Add, remove, export, import bookmarks
5. **View Toggle**: Switch between grid and list views
6. **Year Collapse**: Expand/collapse year sections
7. **Reader Modal**: Open comic, verify iframe loads, close with ESC/click
8. **Responsive**: Test on mobile viewport sizes

### No Automated Tests
This project has no test framework setup. All testing is manual.

---

## Deployment

### GitHub Pages (Recommended)
1. Push to GitHub repository
2. Enable GitHub Pages in repository settings
3. Select source branch (usually `main`)

### Static Hosting
Any static hosting service works:
- Netlify
- Vercel
- Cloudflare Pages
- AWS S3 + CloudFront

### Local File
Can be opened directly as `file://` (though some features may have CORS limitations)

---

## API Reference

### Archive.org Advanced Search
```
https://archive.org/advancedsearch.php?q=identifier:Chandoba_Marathi_*&fl[]=identifier&fl[]=title&fl[]=year&sort[]=year+asc&sort[]=title+asc&output=json&rows=600
```

### Archive.org Embed URL Pattern
```
https://archive.org/stream/{identifier}?ui=embed
```

### Archive.org Thumbnail Pattern
```
https://archive.org/services/img/{identifier}
```

---

## Security Considerations

1. **XSS Prevention**: `escapeHtml()` function sanitizes dynamic content
2. **Iframe Sandbox**: Reader iframe uses `sandbox="allow-scripts allow-same-origin allow-popups"`
3. **No User Data**: Only bookmarks stored locally; no server-side data
4. **External Resources**: Relies on Archive.org and Google Fonts CDNs

---

## Known Limitations

1. **No Server Component**: Cannot proxy or cache Archive.org data
2. **LocalStorage Only**: Bookmarks are device-specific
3. **Archive.org Dependency**: If Archive.org is down, only thumbnails fail gracefully
4. **No User Accounts**: No cross-device bookmark synchronization
5. **Static Data**: Comic list is hardcoded (years 1960-2006)

---

## Future Enhancement Ideas

- Add service worker for offline capability
- Implement IndexedDB for larger local storage
- Add share functionality (Web Share API)
- Add keyboard shortcuts for navigation
- Implement dark/light theme toggle
- Add PDF download links where available
