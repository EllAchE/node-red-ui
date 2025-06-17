# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Node-RED is a low-code programming tool for event-driven applications built on Node.js. This is a monorepo containing multiple packages under the `@node-red` scope, assembled into the main `node-red` module.

## Architecture

The codebase is organized as a monorepo with packages in `packages/node_modules/@node-red/`:

- **@node-red/runtime** - Core runtime engine that executes flows
- **@node-red/editor-api** - Express application serving the editor and Admin HTTP API
- **@node-red/editor-client** - Frontend client resources (HTML, CSS, JavaScript)
- **@node-red/nodes** - Default set of core nodes (inject, debug, function, etc.)
- **@node-red/registry** - Internal node registry for managing installed nodes
- **@node-red/util** - Common utilities shared across modules
- **node-red** - Main entry point that orchestrates all modules

## Development Commands

### Building
- `npm run build` - Full production build with minification
- `npm run build-dev` - Development build without minification
- `grunt build` - Same as npm run build
- `grunt build-dev` - Same as npm run build-dev

### Development Server
- `npm run dev` - Start development server with file watching and auto-restart
- `npm start` - Start production server
- `grunt dev` - Same as npm run dev (uses nodemon + concurrent watching)

### Testing
- `npm test` - Runs all tests (equivalent to `grunt` default task)
- `grunt` - Full test suite: build + code style + unit tests with coverage
- `grunt no-coverage` - Same as above but without code coverage
- `grunt test-core` - Test only core runtime code
- `grunt test-nodes` - Test only core nodes
- `grunt test-editor` - Code style check on editor code only
- `grunt test-ui` - UI tests (requires separate UI test dependencies)

### Code Quality
- `grunt jshint:editor` - Lint editor JavaScript files
- `grunt jshint:nodes` - Lint core node files
- `grunt jsonlint` - Validate JSON files (locales, keymaps)

### Individual Build Tasks
- `grunt concat` - Concatenate editor JavaScript files
- `grunt uglify` - Minify JavaScript files
- `grunt sass` - Compile SCSS to CSS
- `grunt copy:build` - Copy static assets to build directory

## Testing Framework

Tests use Mocha with should.js assertions:
- Unit tests: `test/unit/**/*_spec.js`
- Node tests: `test/nodes/**/*_spec.js` 
- UI tests: `test/editor/**/*_uispec.js` (WebDriver-based)

## Editor Client Build Process

The editor client has a complex build process that concatenates JavaScript files in a specific order (see Gruntfile.js concat:build task). Key files are processed in dependency order:

1. Polyfills and jQuery addons
2. Core RED object and framework
3. UI components (common widgets, then specific UI panels)
4. Editor and node management
5. Projects and tours

SCSS files are compiled from `packages/node_modules/@node-red/editor-client/src/sass/` to minified CSS.

## Code Style

- 4-space indentation (no tabs)
- Opening braces on same line as if/for/function
- Apache 2.0 license header required on all files
- JSHint configuration in `.jshintrc`
- ES2020 features allowed (esversion: 11)

## Localization

Internationalization files are in `locales/` directories under each package. JSON files must pass `grunt jsonlint` validation.

## Node Structure

Core nodes are in `packages/node_modules/@node-red/nodes/core/` organized by category:
- `common/` - Basic nodes (inject, debug, catch, etc.)
- `function/` - Logic nodes (function, switch, change, etc.) 
- `network/` - Communication nodes (HTTP, MQTT, WebSocket, etc.)
- `parsers/` - Data format nodes (JSON, XML, CSV, etc.)
- `sequence/` - Flow control nodes (split, join, batch, etc.)
- `storage/` - File system nodes

Each node consists of:
- `.html` file - Node definition, help text, and editor UI
- `.js` file - Node runtime behavior

## Development Workflow

1. Make changes to source files in `src/` directories
2. Use `npm run dev` for live development with auto-rebuild
3. Run `npm test` before committing to ensure all tests pass
4. Follow the 4-space indentation and coding standards
5. Target the correct branch: `master` for stable fixes, `dev` for new features