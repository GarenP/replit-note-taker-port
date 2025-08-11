# Replit.md

## Overview

This is a 3D knowledge graph visualization application built with React and Express, inspired by the pretty-mindmap-haven library. The application displays notes as interactive 3D nodes in a force-directed graph, allowing users to explore connections between different pieces of knowledge. Users can click on nodes to view detailed note information in a preview panel. The app now features a beautiful file explorer sidebar for managing folders and notes hierarchically.

## User Preferences

Preferred communication style: Simple, everyday language.

## Recent Changes

- **July 17, 2025**: Real-time Force Parameter Updates
  - **Problem**: Node distance slider wasn't updating graph forces in real-time
  - **Implementation**: Added separate useEffect to update d3Force parameters without graph recreation
  - **Issue Found**: Disconnected nodes drift apart when updating forces due to charge force changes
  - **Solution**: Only update link distance force, keep charge force constant to prevent unlinked node drift
  - **Performance**: Smooth real-time updates for linked nodes without affecting isolated nodes

- **July 17, 2025**: Fixed Graph Refreshing/Glitching Issue
  - **Problem**: Graph was refreshing/glitching when clicking nodes or editing notes
  - **Root Cause**: Graph component was recreating entirely on every React re-render
  - **Solution**: Implemented proper memoization with React.memo
    - Added intelligent comparison to check only structural changes (titles, categories, wiki links)
    - Memoized callback functions (handleNodeClick, handleHomeCameraRef) to prevent re-renders
    - Separated node/particle size updates from graph recreation
    - Graph now only recreates when notes structure actually changes, not on content edits
  - **Performance**: Smooth node clicks and camera animations without graph refreshing

- **July 16, 2025**: Advanced Tiptap Editor Integration with Minimalistic Design
  - **Tiptap Editor**: Replaced basic editor with advanced WYSIWYG editor from user's note app
    - Rich toolbar with formatting buttons (bold, italic, headers, lists, etc.)
    - Sticky toolbar that remains visible while scrolling
    - Light blue button styling with hover effects
    - Proper markdown-to-HTML conversion for display and HTML-to-markdown for storage
    - Auto-save functionality with 500ms debounce
    - Single-view editing (no split-pane as requested)
  - **Minimalistic Design**: Reduced excessive padding throughout interface
    - Note preview padding reduced from px-8 py-4 to p-3
    - Title font size reduced from 2xl to xl with tighter margins
    - Compact badge spacing and smaller gaps
    - Top bar height reduced with smaller buttons (h-8→h-6) and icons (h-4→h-3)
    - Toolbar padding reduced and editor content padding optimized
    - Tag/badge size reduced by 50% (padding, font size, and margins)
    - All formatting preserved with proper colors and spacing
  - **Technical**: Fixed React warnings and added proper CSS styling

- **July 16, 2025**: Camera Position Preservation and Improved Node Distance Control
  - **Camera Position Fix**: Graph now preserves camera position when adjusting settings
    - Camera position and zoom level are saved before updating graph properties
    - Position is restored after changes, preventing unwanted zoom resets
    - Separated node/particle size updates from graph recreation for better performance
  - **Refined Node Distance Control**: 
    - Reduced maximum distance from 200 to 30 for much tighter node clustering
    - Range now 5-30 with step size of 1 for precise control
    - Default distance set to 15 for optimal initial spacing
    - Implemented 300ms debouncing to smooth slider updates
  - **Previous**: Gravity Control for Node Distance
    - Added gravity/distance control slider
    - Removed time-based growth animation per user request
    - Focus on static visualization with adjustable node spacing

- **July 15, 2025**: Final Editor Implementation - Obsidian-style Markdown Editor
  - **ARCHITECTURAL CHANGE**: Replaced all previous editors with @uiw/react-md-editor
    - Implemented true Obsidian-style markdown editor with live preview
    - Removed separate edit/view modes - now always editable
    - Split-pane editor with write and preview sections
    - Full dark theme matching Obsidian's aesthetic
  - **Editor Features**: Complete markdown editing experience
    - Live preview with syntax highlighting
    - Supports all markdown features: headers, lists, code blocks, tables
    - Maintains wiki links [[]] and image embeds ![[]] 
    - Toolbar with formatting shortcuts
    - Auto-saves content as you type
  - **Previous Attempts**: 
    - Editor.js (removed - user didn't like it)
    - BlockNote (removed - user wanted true markdown editor)
    - Custom contentEditable solutions (abandoned)

- **July 15, 2025**: Fixed note embedding functionality with ![[]] syntax
  - **Note Embedding**: Implemented working ![[Note Title]] syntax for embedding notes
    - Text notes are embedded with blue left border and proper indentation (no italic formatting)
    - Image notes are embedded as clean images without borders or titles
    - Embedded content displays the full note content inline within the parent note
    - Simplified processing approach using direct content replacement before ReactMarkdown
    - Fixed edit mode functionality to properly save note changes
  - **Performance**: Maintained optimized bulk operations from previous updates
    - Batch processing for multiple note operations
    - Single query cache invalidation after bulk operations complete

- **July 14, 2025**: Performance optimization for bulk operations
  - **Batch Operations**: Implemented batching for bulk note creation and deletion
    - Disabled automatic query invalidation during bulk operations
    - Graph now only refreshes once after all operations complete
    - Significantly reduced lag when uploading/deleting multiple notes
  - **Optimized File Upload**: Enhanced markdown and image file processing
    - Direct API calls instead of mutation hooks during bulk uploads
    - Single query cache invalidation after all files processed
    - Progress notifications maintained without triggering graph refreshes
  - **Bulk Delete**: Improved multi-select deletion performance
    - Direct API calls for each deletion without individual cache updates
    - Final cache invalidation only after all deletions complete

- **July 14, 2025**: Major enhancements to note preview and image support
  - **Persistent Window Position**: Note preview window position and size are now saved to localStorage and persist across sessions
  - **Edit/View Mode**: Implemented Obsidian-style edit and view modes for notes
    - Toggle between modes with Edit/Eye button in the note preview header
    - Edit mode shows a textarea with raw markdown content
    - View mode renders formatted markdown with live preview
  - **Image Support**: Full implementation of image storage and display
    - Images can be uploaded through the file explorer (supports png, jpg, jpeg, gif, webp, svg)
    - Images are stored as base64 in the database with proper MIME types
    - Image notes display with an image icon in the file explorer
    - Images can be embedded in notes using Obsidian's ![[image name]] syntax
    - Embedded images display inline within notes in view mode
    - Image embed syntax is hidden when not hovering (Obsidian-style behavior)
  - **Database Schema Updates**: Added support for image storage
    - Added `type` field to notes table ('text' or 'image')
    - Added `imageData` field for base64 encoded image data
    - Added `mimeType` field for proper image format handling
  - **Enhanced Markdown Rendering**: Improved wiki link and image embed display
    - Wiki links show brackets only when hovering over them
    - Non-existing wiki links remain visible with gray styling
    - Image embeds are processed with custom remark plugin

- **July 14, 2025**: Added draggable and resizable note preview window
  - Note preview window is now fully draggable by clicking and dragging the header
  - Added resize handles on all corners and edges for flexible window sizing
  - Window can be moved anywhere on the screen, including over the left panel
  - Minimum window size constraints (300x400) to maintain usability
  - Smooth resize and drag interactions with proper cursor indicators
  - Window maintains position and size during navigation between notes
  - Blue resize handles appear on hover for visual feedback
  - Enhanced window styling with rounded corners and shadow
  - Fixed resize anchoring so window edges stay fixed when resizing opposite edges
  - Added complete boundary constraints to prevent window from disappearing off-screen
  - Window now properly stays within viewport bounds during all resize and drag operations

- **July 14, 2025**: Fixed wiki link functionality to work exactly like Obsidian
  - Wiki links with [[]] syntax now work properly in all notes
  - Fixed remark plugin to correctly process wiki links and convert them to clickable links
  - Existing notes show as blue links without brackets (Obsidian-style)
  - Non-existing notes show as dark gray links with brackets still visible
  - Clicking non-existing links automatically creates new notes with that title
  - Added proper URL encoding/decoding for note titles with spaces
  - Enhanced CSS styling for wiki links to match Obsidian's appearance
  - Fixed navigation history to work properly with wiki link clicks
  - Added auto-creation functionality for missing notes when wiki links are clicked

- **July 14, 2025**: Enhanced wiki link navigation and markdown formatting consistency
  - Fixed URL encoding issues in wiki links (spaces now properly decoded)
  - Implemented proper back/forward navigation history in note preview
  - Added small minimalistic navigation buttons (← →) in top-left of note preview
  - Standardized markdown formatting across all notes (proper numbered lists, line breaks)
  - Wiki links now properly separated on individual lines for better readability
  - Navigation history persists across wiki link clicks without resetting
  - Fixed graph refresh button to return notes instead of 501 error
  - All journal notes now follow consistent markdown formatting standards
  - Implemented multi-select functionality with Ctrl+Click and Shift+Click for range selection
  - Added bulk delete operations with confirmation dialog
  - Created right-click context menu for file operations
  - Added click-outside-to-deselect functionality for better user experience

- **July 13, 2025**: Implemented comprehensive drag and drop functionality for file organization
  - Added multi-select capability with Ctrl+click for selecting multiple notes and folders
  - Implemented drag and drop for moving notes and folders between different locations
  - Created visual feedback system with colored borders for selection and drop zones
  - Added automatic folder expansion when items are dropped into folders
  - Implemented root-level drop support for moving items to top level
  - Added selection counter and user instructions for intuitive operation
  - Fixed data persistence issues ensuring all changes are saved to PostgreSQL database
  - Added sequential processing with delays to prevent race conditions during batch moves

- **July 13, 2025**: Implemented Obsidian-style wiki links and improved file upload system
  - Added Obsidian-style [[]] wiki link functionality with custom remark plugin
  - Created clickable navigation between linked notes in markdown content
  - Added visual feedback for existing vs non-existing wiki links
  - Fixed file upload system to support multiple file selection
  - Increased server payload limit to 50MB for large file uploads
  - Improved upload progress feedback with sequential processing
  - Added batch processing with delays to prevent server overload
  - Enhanced error handling for failed file uploads

- **July 13, 2025**: Major improvements to graph interaction and label display
  - Implemented interactive node dragging with left mouse button
  - Configured right mouse button for view rotation, middle mouse for panning
  - Fixed blurry node titles with high-resolution canvas rendering
  - Made node labels always visible (100% opacity) from startup
  - Simplified graph centering to consistently center at origin
  - Removed label threshold slider - labels now permanently visible
  - Fixed label persistence when adjusting node size settings
  - Added multiple centering attempts to ensure proper graph positioning

## System Architecture

### Frontend Architecture
- **Framework**: React with TypeScript
- **Build Tool**: Vite for fast development and optimized builds
- **UI Components**: Shadcn/UI component library built on Radix UI primitives
- **Styling**: Tailwind CSS with custom CSS variables for theming
- **3D Visualization**: 3D Force Graph library for interactive node visualization
- **State Management**: React Query (@tanstack/react-query) for server state management
- **Routing**: Wouter for lightweight client-side routing

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Database**: PostgreSQL with Drizzle ORM
- **Database Provider**: Neon Database (serverless PostgreSQL)
- **API**: RESTful API with JSON responses
- **Development**: Hot module replacement via Vite integration

## Key Components

### Data Layer
- **Schema**: Centralized schema definition in `shared/schema.ts` using Drizzle ORM
- **Storage**: Abstracted storage interface with in-memory implementation for development
- **Database**: PostgreSQL with connection pooling via Neon Database

### Frontend Components
- **Graph3D**: Main 3D visualization component using Force Graph library
- **NotePreview**: Side panel for displaying detailed note information
- **FileExplorer**: Hierarchical file explorer sidebar for managing folders and notes
- **UI Components**: Comprehensive set of accessible components from Shadcn/UI

### Backend Services
- **Routes**: RESTful endpoints for CRUD operations on notes
- **Storage**: Abstract storage interface with memory-based implementation
- **Database Migrations**: Drizzle Kit for schema management

## Data Flow

1. **Application Start**: React app loads and queries `/api/notes` endpoint
2. **Data Fetching**: React Query manages server state and caching
3. **Visualization**: Notes are transformed into 3D graph nodes with connections
4. **User Interaction**: Clicking nodes triggers note preview display
5. **Data Updates**: Mutations update server state and invalidate cached data

## External Dependencies

### Core Libraries
- **3D Visualization**: `3d-force-graph` and `three.js` for 3D rendering
- **Database**: `@neondatabase/serverless` for PostgreSQL connection
- **ORM**: `drizzle-orm` for type-safe database operations
- **Validation**: `zod` for runtime type validation
- **UI**: Radix UI primitives for accessible components

### Development Tools
- **Build**: Vite with React plugin and TypeScript support
- **Database**: Drizzle Kit for migrations and schema management
- **Styling**: Tailwind CSS with PostCSS processing
- **Development**: ESBuild for server bundling

## Deployment Strategy

### Build Process
- **Client**: Vite builds React app to `dist/public`
- **Server**: ESBuild bundles Express server to `dist/index.js`
- **Database**: Drizzle pushes schema changes to PostgreSQL

### Environment Configuration
- **Database**: Requires `DATABASE_URL` environment variable
- **Development**: Uses NODE_ENV for environment-specific behavior
- **Production**: Serves static files and API from single Express server

### Architecture Decisions

1. **Monorepo Structure**: Single repository with shared schema between client and server
2. **TypeScript**: Full type safety across frontend, backend, and database
3. **Memory Storage**: Development fallback when database is unavailable
4. **3D Visualization**: Chosen for engaging user experience and better data exploration
5. **Serverless Database**: Neon Database for scalable, managed PostgreSQL
6. **Component Library**: Shadcn/UI for consistent, accessible UI components