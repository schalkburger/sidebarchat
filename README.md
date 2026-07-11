# SidebarChat

Open any Discord channel or DM in a side panel or popout window without leaving your current view.

## What's Fixed

The original plugin was broken on current Equicord. Two critical issues resolved:

### 1. React Hooks Crash (#321)
`renderSidebar()` used React hooks (`useState`, `useEffect`, etc.) inside a class component's `render()` method. React requires hooks to be called at the top level of a function component or custom hook — never inside class methods, loops, or conditions.

**Fix:** Extracted all hook logic into a standalone `SidebarView` function component at module level. The original `renderSidebar()` now delegates to this component, keeping hooks on their own fiber tree.

### 2. ChannelSectionStore Crash
The plugin imported `ChannelSectionStore` from `@webpack/common/stores`, but this store doesn't exist in Equicord's current store registry. This threw a runtime error on load.

**Fix:** Wrapped the store lookup in try-catch. The selector gracefully returns `null` when the store is unavailable, and the stores array is filtered to exclude null entries.

### 3. Minor Fixes
- Added `EquicordDevs.darkharden` author to `src/utils/constants.ts`
- Updated plugin authors list

## Installation

1. Copy the `sidebarChat` folder to your Equicord `src/equicordplugins/` directory
2. Add `darkharden` entry to `src/utils/constants.ts` if not present:
   ```ts
   darkharden: {
       name: "darkharden",
       discord: "95976916237975552",
   },
   ```
3. Build Equicord

## Usage

- Click the sidebar icon in the channel header to open a channel as a sidebar
- Drag the sidebar to reposition (left/right)
- Use the popout button for a floating window
- Switch channels directly within the sidebar

## Files

- `index.tsx` — Main plugin logic, patches, and UI components
- `store.ts` — Plugin state management
- `styles.css` — Sidebar and popout styling

## Credits

- **Joona** — Original plugin
- **justjxke** — Original plugin
- **darkharden** — Fixes for current Equicord compatibility
