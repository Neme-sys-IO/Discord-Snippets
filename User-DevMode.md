# Developer/Experimental Mode
![Status](https://img.shields.io/badge/Status-WORKING-brightgreen?style=for-the-badge)
## Overview
This code snippet allows you to grant access to the Experiments and Developer Tools of Discord.

## Usage
> [!TIP]
> Checkout/Create [Issue](https://github.com/Neme-sys-IO/Discord-Snippets/issues) if you get any errors or find any issue even after properly following all the steps.

### For Browsers:
1. Log in to the Discord web application.
2. Open the developer tools by pressing `CTRL + SHIFT + I` or right-clicking on the page and selecting "Inspect".
3. Go to the "Console" tab.
4. Copy the [code](https://github.com/Neme-sys-IO/Discord-Snippets/edit/main/Guild-StaffMode.md#code) and paste it into the console.
5. Hit `Enter` to run the code.

### For PC:
1. Make sure to have Discord updated and logged in.
2. Close/Kill the Discord process from the Background. 
3. Then Navigate to...
- Windows: `\Users\{username}\AppData\Roaming\discord`
- Mac: `/Users/{username}/Library/Application Support/discord`
- Linux: `/home/{username}/.config/discord`
> [!NOTE]
> The "discord" folder directory varies to the client type...
> - For Stable: `discord`
> - For PTB: `discordptb`
> - For Canary: `discordcanary`
> - For Developement: `discorddevelopment`
4. Find & Open `settings.json` in a Notepad and you will see something like this:
```json
{
   "chromiumSwitches": {},
	"OPEN_ON_STARTUP": false,
	"IS_MAXIMIZED": false,
	"IS_MINIMIZED": false,
	"WINDOW_BOUNDS": {
		"x": 320,
		"y": 160,
		"width": 1280,
		"height": 720
	}
}
```
5. Add this below line innit or set it's value to `true`:
```json
"DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT YOURE_DOING": true,
```
6. Save the file and now open Discord client.
7. Press `CTRL + SHIFT + I` or `CMD + OPTION + I`. (This will open Chromium-Based WebDeveloperTools in the client)
8. Copy the [code](https://github.com/Neme-sys-IO/Discord-Snippets/edit/main/Guild-StaffMode.md#code) and paste it into the console.
9. Hit `Enter` to run the code.
### For Mobile
1. Login Discord in any Chromium-based browser.
2. Create a Bookmark in browser with Unique Name and add this in URL:
```javascript
javascript:(()=>{Pasta})()
```
3. Replace `Pasta` with this [code](https://github.com/Neme-sys-IO/Discord-Snippets/edit/main/Guild-StaffMode.md#code) and save it.
4. Visit `https://discord.com/channels/@me`.
5. Open the search bar and type that Unique Name of that Bookmark.
6. If it appear in the search suggestion, hit it and it'll exucute the code. If not, rename it to something even more unique.
> [!WARNING]
> This will probably won't work in Firefox-based browsers and others.
## Code
```javascript
{
    let c = webpackChunkdiscord_app.push([[Symbol()], {}, r => r.c]);
    webpackChunkdiscord_app.pop();

    let u = Object.values(c).find(x =>
        x?.exports?.default?.__proto__?.getUsers &&
        x?.exports?.default?.getCurrentUser
    ).exports.default;

    let m = Object.values(u._dispatcher._actionHandlers._dependencyGraph.nodes);

    u.getCurrentUser().flags |= 1;

    m.find(x => x.name === "DeveloperExperimentStore")
     .actionHandler["CONNECTION_OPEN"]();

    try {
        m.find(x => x.name === "ExperimentStore")
         .actionHandler["OVERLAY_INITIALIZE"]({ user: { flags: 1 } });
    } catch {}

    m.find(x => x.name === "ExperimentStore")
     .storeDidChange();
}
```
