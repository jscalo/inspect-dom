# inspect-dom

Inspect DOM elements and their styles from Chrome using the Chrome DevTools Protocol.

## Description

`inspect-dom` connects to Chrome's remote debugging interface to inspect DOM elements and their CSS styles. It provides a command-line interface similar to Chrome DevTools for analyzing web page elements, their structure, and styling information.

## Prerequisites

Chrome must be launched with remote debugging enabled:

```bash
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222
```

## Installation

```bash
npm install
```

## Usage

```bash
inspect-dom <selector> [options]
```

### Arguments

- `<selector>` - CSS selector for the element to inspect (e.g., #myId, .myClass, div.container)
  - Use quotes for complex selectors with spaces or special characters

### Options

- `-r, --recursive` - Include child elements (default: only show matching element)
- `--styles` - Show declared CSS styles for each element
- `--computed-styles` - Show computed CSS styles for each element
- `--no-defaults` - Hide browser default/user-agent styles (only with --styles)
- `--debug` - Show verbose debugging information (for troubleshooting only)
- `-h, --help` - Show help message

## Examples

### Basic DOM inspection
```bash
inspect-dom #header
```
Show only the element with id "header"

### Recursive inspection
```bash
inspect-dom .sidebar -r
```
Show element with class "sidebar" and all its children

### Style inspection
```bash
inspect-dom div.main-content --styles
```
Show element and its declared styles (no children)

### Computed styles
```bash
inspect-dom .container -r --computed-styles
```
Show element and all children with computed styles

### Combined style inspection
```bash
inspect-dom .nav -r --styles --computed-styles
```
Show element and children with both declared and computed styles

### Exclude browser defaults
```bash
inspect-dom .container --styles --no-defaults
```
Show element styles excluding browser defaults

### Debug mode
```bash
inspect-dom .container --styles --debug
```
Show element with verbose debugging information (for troubleshooting connection issues)

### Complex selectors (quotes required)
```bash
inspect-dom "div .container" --styles
```
Show descendant selector (quotes needed for spaces)

```bash
inspect-dom "input[type='text']" --styles
```
Show attribute selector (quotes needed for special characters)

## Features

- **DOM Structure Display**: Shows hierarchical DOM structure with tag names, IDs, and classes
- **Style Analysis**: Displays CSS styles in Chrome DevTools format
- **Specificity Sorting**: Orders CSS rules by specificity (highest first)
- **Inheritance Display**: Shows inherited styles from parent elements
- **Stylesheet Mapping**: Attempts to map styles to their source files with line numbers
- **Cross-tab Detection**: Automatically finds the correct Chrome tab to inspect

## Output Format

The tool provides structured output including:

1. **DOM Structure**: Hierarchical display of elements
2. **Style Information** (when requested):
   - Inline styles (highest priority)
   - Stylesheet styles (sorted by specificity)
   - Inherited styles from parent elements

## Requirements

- Node.js
- Chrome browser with remote debugging enabled
- macOS (uses AppleScript for tab detection)

## Dependencies

- `chrome-remote-interface` - For connecting to Chrome's debugging protocol