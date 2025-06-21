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
- `--show-implicit-styles` - Include browser-computed and implicit CSS properties (default: explicit styles only)
- `--debug` - Show verbose debugging information (for troubleshooting only)
- `-h, --help` - Show help message

## Examples

### Typical usage

```bash
inspect-dom .bottom-nav-bar -r --styles
```

```
--- DOM Structure ---

<div class="bottom-nav-bar">
  <button class="nav-back-button">
    <img src="/assets/nav-back.svg" alt="Back" />
  </button>
  <div class="nav-center-labels">
    <div class="nav-product-name"></div>
    <div class="nav-price"></div>
  </div>
  <button class="nav-add-to-cart-button"></button>
</div>

--- Style Information ---

<div class="bottom-nav-bar">:
  Styles:
    Stylesheet Styles:
      .bottom-nav-bar - styles.css:95:
        position: fixed
        bottom: 0px
        left: 0px
        right: 0px
        height: 77px
        background-color: rgba(255, 255, 255, 0.75)
        display: flex
        align-items: center
        z-index: 1000
    Inherited Styles:
      From ancestor level 1:
        html, body:
          margin: 0
          margin-top: 0px
          margin-right: 0px
          margin-bottom: 0px
          margin-left: 0px

  <button class="nav-back-button">:
    Styles:
      Inline Styles:
        style="transform: scale(1); transform-origin: left center;"
          transform: scale(1)
          transform-origin: left center
...
```

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

### Include implicit styles
```bash
inspect-dom .container --styles --show-implicit-styles
```
Show element styles including browser-computed and implicit properties

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
- **Stylesheet Mapping**: Maps styles to their source files with line numbers

## Output Format

The tool provides structured output including:

1. **DOM Structure**: Hierarchical display of elements
2. **Style Information**:
   - Inline styles (highest priority)
   - Stylesheet styles (sorted by specificity)
   - Inherited styles from parent elements
   - **Note**: By default, only explicit CSS declarations are shown. Use `--show-implicit-styles` to include browser-computed defaults.

## Requirements

- Node.js
- Chrome browser with remote debugging enabled
- macOS (uses AppleScript for tab detection)

## Dependencies

- `chrome-remote-interface` - For connecting to Chrome's debugging protocol