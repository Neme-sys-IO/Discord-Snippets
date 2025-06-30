# Discord Staff Guild Access Snippet
![Status](https://img.shields.io/badge/Status-WORKING-brightgreen?style=for-the-badge)
## Overview

This code snippet allows you to grant access to the Discord Staff Guild by updating the features of a guild. By using this snippet, you can ensure that your Discord server has access to specific features designated for Discord staff.

## Usage
1. **Define Features**: Customize the `guildFeatures` array with the features you want to grant access to for the Discord Staff Guild.

2. **Run the Snippet**:
   - Open your web browser.
   - Log in to the Discord web application and navigate to any server.
   - Open the developer tools by pressing `F12` or right-clicking on the page and selecting "Inspect".
   - Go to the "Console" tab.
   - Copy the code snippet and paste it into the console.
   - Press `Enter` to run the code.

## Important Note

Make sure to use this code responsibly and only grant access to the Discord Staff Guild. Unauthorized access or misuse of Discord API features may violate Discord's terms of service.

## Code

```javascript
let moduleCache;
webpackChunkdiscord_app.push([
    [Symbol()],
    {},
    (m) => {
        moduleCache = m.c;
    }
]);
let _mods = webpackChunkdiscord_app.push([
    [Symbol()],
    {},
    (m) => m.c
]);
webpackChunkdiscord_app.pop();
const findByProps = (...props) => {
    for (let m of Object.values(_mods)) {
        try {
            if (!m.exports || m.exports === window) continue;
            if (props.every((x) => m.exports?.[x])) return m.exports;
            for (let ex in m.exports) {
                if (
                    props.every((x) => m.exports?.[ex]?.[x]) &&
                    m.exports[ex][Symbol.toStringTag] !== "IntlMessagesProxy"
                ) {
                    return m.exports[ex];
                }
            }
        } catch {}
    }
};
const find =
    typeof findByProps !== "undefined" && findByProps ||
    window?.findByProps ||
    window?.Vencord?.Webpack?.findByProps;
if (!find) {
    throw void console.log(
        "%cUh huh... This script seems to be OUTDATED.",
        "color:red;font-size:2rem"
    );
}
const PermissionStore = find(
    "can",
    "canBasicChannel",
    "canAccessGuildSettings",
    "canImpersonateRole"
);
const UserStore = find("getUserStoreVersion");
const Guild = Object.values(find("getGuildCount").getGuilds())[0];
const permissionMethods = [
    "can",
    "canAccessMemberSafetyPage",
    "canAccessGuildSettings",
    "canBasicChannel",
    "canImpersonateRole",
    "canManageUser",
    "canWithPartialContext",
    "getGuildVersion",
    "getChannelsVersion",
    "getChannelPermissions",
    "getHighestRole",
    "initialize",
    "constructor",
    "isRoleHigher"
];
if (PermissionStore) {
    permissionMethods.forEach(method => {
        PermissionStore.__proto__[method] = () => true;
    });
}
const setProtoFields = (obj, fields, value) => {
    fields.forEach(field => {
        Object.getPrototypeOf(obj)[field] = value;
    });
};
setProtoFields(PermissionStore, [
    "can",
    "canAccessGuildSettings",
    "canAccessMemberSafetyPage",
    "canBasicChannel",
    "canImpersonateRole",
    "canManageUser",
    "canWithPartialContext",
    "isRoleHigher"
], () => true);
setProtoFields(Guild, [
    "isOwner",
    "isOwnerWithRequiredMfaLevel"
], function(id) {
    return [UserStore.getCurrentUser()?.id, this.ownerId].includes(id);
});
```
