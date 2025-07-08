# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

### Essential Commands
- `npm install && npm run setup` - Install dependencies and setup addons
- `npm run build` - Build TypeScript files (runs `tsc`)
- `npm run watch` - Watch mode for TypeScript compilation
- `npm run esbuild` - Bundle demo application
- `npm run esbuild-watch` - Watch mode for bundling
- `npm run package` - Create production build (builds + webpack + esbuild)

### Testing
- `npm run test` - Run unit tests (includes linting)
- `npm run test-unit` - Run unit tests only
- `npm run test-unit-coverage` - Run unit tests with coverage
- `npm run test-integration` - Run Playwright integration tests
- `npm run test-integration-debug` - Debug integration tests (single worker, headed)
- `npm run lint` - ESLint for TypeScript files
- `npm run lint-api` - ESLint for type definition files

### Development Server
- `npm start` - Start demo server (runs `node demo/start`)

## Project Architecture

### Core Structure
This is a monorepo containing the main xterm.js library and multiple addons:

- **`src/`** - Main library source code
  - **`src/browser/`** - Browser-specific implementation (main Terminal class)
  - **`src/common/`** - Shared code between browser and headless
  - **`src/headless/`** - Node.js headless terminal implementation
  - **`src/vs/`** - Visual Studio Code utilities (scrollbar, events, etc.)

- **`addons/`** - Separate addon packages (each with own package.json)
  - Built independently but share dependencies with main project
  - Examples: `addon-fit`, `addon-webgl`, `addon-search`, `addon-clipboard`

### Build System
- **TypeScript Project References**: Uses `tsconfig.all.json` to orchestrate building all projects
- **Webpack**: Production bundling to create UMD library in `lib/xterm.js`
- **ESBuild**: Alternative fast bundling (used for demo and development)
- **Output Structure**:
  - `out/` - TypeScript compilation output
  - `lib/` - Final webpack bundles for distribution

### Key Components
- **Terminal Class** (`src/browser/public/Terminal.ts`) - Main public API
- **CoreTerminal** (`src/common/CoreTerminal.ts`) - Core terminal logic
- **InputHandler** (`src/common/InputHandler.ts`) - Escape sequence processing
- **Buffer System** (`src/common/buffer/`) - Terminal buffer management
- **Renderers** (`src/browser/renderer/`) - DOM and WebGL rendering
- **Parser** (`src/common/parser/`) - VT sequence parsing

### Addon System
Addons extend terminal functionality and are loaded via `Terminal.loadAddon()`. Each addon:
- Has its own TypeScript project with separate compilation
- Shares type definitions with core terminal
- Can hook into terminal events and provide new capabilities

### Testing Strategy
- **Unit Tests**: Mocha-based tests for individual components
- **Integration Tests**: Playwright tests for full browser functionality
- **Benchmark Tests**: Performance testing for critical paths

## Development Notes

- The project uses TypeScript project references for efficient compilation
- Addons are automatically installed via `bin/install-addons.js`
- The demo application showcases terminal features and can be used for testing
- WebGL rendering is available as an addon for performance-critical applications
- The terminal supports both browser and Node.js (headless) environments