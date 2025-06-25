# macOS-Show-Desktop-Toggle-Minimize-and-Restore-Visible-Apps-Using-Hammerspoon
A smart Show Desktop toggle for macOS that minimizes all currently visible windows and restores them later. Built using Hammerspoon and HyperKey for fast, scriptable workspace control.


🙌 Credits

Made by [Sukeshwar Sivakumar]
GitHub: [github.com/yourusername](https://github.com/SukeshSiva)


# 💻 macOS Show Desktop Toggle – Minimize and Restore Visible Apps Using Hammerspoon

A smart and scriptable “Show Desktop” feature for macOS that lets you **minimize all currently visible windows** — and **restore them with a single shortcut**. Unlike macOS’s built-in gestures, this toggle remembers only the windows you minimized and ignores ones that were already hidden. Built using [Hammerspoon](https://www.hammerspoon.org/) and HyperKey.

---

## ✨ Features

- ✅ Minimizes all currently visible app windows
- ➕ Dynamically adds newly opened apps to the minimized list
- 🔁 Press the shortcut again when no windows are visible to restore all previously minimized apps
- 🚫 Leaves already-minimized windows untouched
- ⚡ Fast and lightweight (powered by Hammerspoon)

---

## 🛠 Requirements

- **macOS** (tested on Monterey, Ventura, Sonoma)
- [**Hammerspoon**](https://www.hammerspoon.org/) – automation engine for macOS
- **HyperKey** setup (optional but recommended – via Karabiner-Elements or HyperKey.app)

---

## ⚙️ Setup

### 1. Install Hammerspoon
Download and install from: [https://www.hammerspoon.org/](https://www.hammerspoon.org/)

Grant it **Accessibility permissions** in System Settings when prompted.

### 2. Add the Script
1. Open Hammerspoon and click **“Open Config”**
2. Paste the code from `init.lua` into the file
3. Save and reload (use the Hammerspoon menu → “Reload Config”)

---

## ⌨️ Hotkey

By default, the script binds the toggle to:
Hyper + D  =  Ctrl + Option + Command + Shift + D

If you use a HyperKey (e.g., remapped Caps Lock), this means:
Caps Lock + D


You can customize the hotkey in the last line of the script:

```lua
hs.hotkey.bind({ "ctrl", "alt", "cmd", "shift" }, "D", handleDesktopToggle)



💡 How It Works
	1.	Press Hyper + D
→ All currently visible app windows are minimized and saved to a list
	2.	Open a new app and press the hotkey again
→ That app is also minimized and added to the list
	3.	When no windows are visible, press again
→ All previously minimized apps are restored
	4.	The list is reset, and the cycle begins again




🧩 Code Overview


-- Stores all minimized windows
local restoreList = {}

-- Checks if any visible windows are on screen
local function anyWindowVisible()
    for _, win in ipairs(hs.window.visibleWindows()) do
        if win:isStandard() and not win:isMinimized() then
            return true
        end
    end
    return false
end

-- Main toggle function: minimize or restore
local function handleDesktopToggle()
    if anyWindowVisible() then
        for _, win in ipairs(hs.window.visibleWindows()) do
            if win:isStandard() and not win:isMinimized() then
                win:minimize()
                table.insert(restoreList, win)
            end
        end
    else
        for _, win in ipairs(restoreList) do
            if win:isMinimized() then
                win:unminimize()
            end
        end
        restoreList = {} -- Reset list
    end
end

-- Bind to Hyper + D
hs.hotkey.bind({ "ctrl", "alt", "cmd", "shift" }, "D", handleDesktopToggle)


