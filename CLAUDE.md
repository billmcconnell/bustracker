# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BusTracker is a single-page web application that visualizes the MBTA 1 Bus route between MIT and Harvard in Cambridge, MA. It animates a map marker along real bus stop coordinates using Mapbox GL for interactive 3D mapping.

## Development Setup & Commands

This project has **no build process, bundlers, or package managers**. All dependencies (Mapbox GL JS) load via CDN.

### Running Locally
```bash
python3 -m http.server 8000  # Serve at http://localhost:8000
# Or use any HTTP server (node's http-server, etc.)
```

Open `index.html` in a browser or navigate to the local server URL. Click "Show stops between MIT and Harvard" to start the animation.

### No Testing Framework
There are no automated tests. Manual testing is performed by:
- Opening the application in a browser
- Clicking the button and verifying the marker animates through bus stops correctly
- Each stop displays for 1 second
- Refreshing the page to restart

## Architecture

### Core Files
- **index.html**: Single HTML file with embedded button overlay (`onclick="move()"`)
- **mapanimation.js**: Map initialization, marker creation, and animation logic
- **styles.css**: Minimal absolute positioning for full-screen map with overlay button

### Data Flow
1. User clicks "Show stops between MIT and Harvard" button
2. `move()` function recursively iterates through `busStops` array coordinates
3. `marker.setLngLat()` updates marker position every 1000ms (1 second)
4. Animation completes when counter reaches array length
5. Page must be refreshed to animate again

### Key Technical Details

**Bus Stop Coordinates**: Stored as `[longitude, latitude]` pairs in `busStops` array (mapanimation.js:2-15). Real coordinates from actual MBTA route. Maintain format when adding new stops.

**Mapbox Integration**:
- Access token embedded in mapanimation.js:18 (tied to billmcconnell Mapbox account)
- Style: `mapbox://styles/mapbox/streets-v11` with 3D buildings layer
- Map centered at [-71.104081, 42.365554], zoom 15.5, pitch 45°, bearing -17.6°
- Token should not be committed to public repos; regenerate via Mapbox Studio for production

**Animation Mechanism**: The `move()` function uses `setTimeout()` recursion (not intervals) to ensure sequential execution and prevent race conditions. Each call increments `counter` to track progress through `busStops` array.

**CSS Model**: Uses absolute positioning with `#map` filling entire viewport and `.map-overlay` elements layering on top. Keep this minimal positioning model intact when styling.

## Planned Enhancements

From README.md—prioritize completing features one at a time:
- Automated page refresh after animation completes
- Stop name banners overlaid on map
- Multiple bus markers with capacity information
- Real-time MBTA API integration

## Notes for Claude
<!-- Specific instructions or context for Claude when working on this codebase -->
1. Act as a Senior Developer would.
2. First think through the problem, read the codebase for relevant files, and write a plan to tasks/todo.md.
3. The plan should have a list of todo items that you can check off as you complete them
4. Before you begin working, check in with me and I will verify the plan.
5. Then, begin working on the todo items, marking each todo item as complete as you go.
6. Provide a high level explanation of the changes you made.
7. Make every task and code change you do as simple as possible. Your goal is to avoid making any large or complex changes. Every change should impact as little existing code as possible.
8. Finally, add a review section to the todo.md file with a summary of the changes you made and any other relevant information.
9. Fix the root cause of any bugs you find. Do not make any temporary fixes to any bug. Ensure that bug fixes impact just the code that needs to be changed and nothing else.
