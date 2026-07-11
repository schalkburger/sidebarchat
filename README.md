# sidebarChat

## The fixes

The original plugin was completely broken on recent Equicord builds. I got it working again by tackling two main crashes:

### 1. React hooks crash

The plugin was trying to use hooks (`useState`, `useEffect`) directly inside a class component's `render()` method. React throws a fit if you do this because hooks have to stay at the top level of function components.

To fix it, I pulled all the hook logic out into a separate `SidebarView` function component. The old `renderSidebar()` method now just handles the rendering, letting the hooks run safely on their own fiber tree.

### 2. Missing ChannelSectionStore

The plugin was looking for `ChannelSectionStore` in the common stores registry, but Equicord doesn't include it anymore. This was causing an immediate runtime crash when the plugin tried to load.

I wrapped the store lookup in a try-catch block. The selector now defaults to `null` if the store is missing, and I added a filter to drop any null values so it doesn't break the rest of the array.

### Minor changes

* Added the author entry to `constants.ts`.
* Updated the credit list.

## Getting it installed

1. Drop the `sidebarChat` folder straight into your `src/equicordplugins/` directory.
2. Make sure the author entry is in `src/utils/constants.ts`:
```ts
darkharden: {
    name: "darkharden",
    discord: "95976916237975552",
},

```


3. Rebuild Equicord.

## How to use it

* Click the new sidebar icon in the channel header to open that chat on the side.
* Drag the panel edge to switch it between the left and right sides of your client.
* Use the popout button to open the channel in a dedicated, floating window.
* You can switch channels right inside the sidebar itself.

## Project structure

* `index.tsx` — All the main plugin logic, patches, and UI views.
* `store.ts` — State management.
* `styles.css` — Layout and popout styles.

## Credits

* **Joona** & **justjxke** — Original plugin design and code.
* **darkharden** — Compatibility updates for current Equicord.
