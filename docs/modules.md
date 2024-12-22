<p align="center">
    <img src="../images/Logo.png">
</p>

# Empower your orbit.

![Current Version](https://img.shields.io/badge/Version-v0.0%20Lockheed%20Martin-2650d0)
[![Terra's Roblox Profile](https://img.shields.io/badge/Developer's-Profile-2650d0)](https://www.roblox.com/users/32573334/profile)
[![Mailto Email Link](https://img.shields.io/badge/Contact-Us-2650d0)](mailto:ebgui.staff@gmail.com)
[![Eos Development Discord Link](https://img.shields.io/badge/Eos%20Development-Discord-2650d0)](https://discord.gg/z3QZzFJBvj)

Comet, created by Eos Development, is the sole top internal user interface for [Gamer Robot's Elemental Battlegrounds](https://www.roblox.com/games/566399244/SOLAR-Elemental-Battlegrounds). With an all-new, fully modular, optimized approach to functions thought static and stagnant before, Comet breathes new life into your favorite game like nothing ever seen before.

---

## Introduction

One of the primary improvements Comet makes to the Elemental Battlegrounds space is its modular nature, and alongside that comes the ability to add your own menus and features. While it's not possible to move or edit functions or features that are already included, this will allow you to write your own features for Comet and distribute them as you like!

## Disclaimer

Comet, while not intentionally designed for it, does allow the modification of data (UI or user) through its features. The ability of doing so is not officially supported, however; perform these actions at your own risk.

---

There are a few methods and functions to go over when it comes to adding your own features. Each method was designed with ease of access in mind; it simply takes a little bit of getting used to.

## Notify

```lua
Comet:Notify({args}): void
```
Sends a notification.

### Parameters

<sup>These parameters should be placed in a table, then provided as one argument.</sup>

- `Message: string` - Required. The notification to send.
- `Error: boolean` - Optional. Determines if an error sound plays.
- `Sound: boolean` - Optional. Determines if a sound plays at all. If provided, will override `Error`.
- `Time: number` - Optional. Determines how long the notification should stay before being recalled.
- `NoRecall: boolean` - Optional. Determins if the notification should recall at all. If provided, will override `Time`.

### Example

```lua
-- Single argument syntax
Comet:Notify(
    {
        Message = "Hello World!",
        Error = true,
        Time = 5
    }
)
```
```lua
-- Table syntax
Comet:Notify{
    Message = "This is a test!",
    Sound = false,
    NoRecall = true
}
```

## SaveData
<sup>[`Yields`](## "This method will yield the current thread") [`File management`](## "This method managed files and should be handled carefully") </sup>

```lua
Comet:SaveData(filePath: string,data: table): void
```

Saves the information in table `data` to a JSON file located at `filePath`, whose root is in the executor's Workspace.

### Parameters

- `filePath: string` - The path to the file, rooted at the executor's Workspace. Defaults to Comet's `UserSettings.json` file.
- `data: table` - The data to encode and save as JSON. Defaults to the current user data for Comet.

### Example

```lua
local d = {
    your = "mom",
    is = {"fat","lol"},
    nerd = true
}
Comet:SaveData("Comet\\Plugins\\HelloWorld.json",d)
```
## AddMenu
<sup>[`Yields`](## "This method will yield the current thread")</sup>

```lua
Comet:AddMenu(name: string,isScrolling: boolean): boolean?
```
Adds a menu to the sidebar of Comet.

### Parameters

- `name: string` - The name of your menu. This is what will be referred to internally, and is also what the button in the sidebar will read. Will force `AddMenu` send an error notification and return `false` if omitted or not a string.
- `isScrolling: boolean` - Whether you want the menu to be a `ScrollingFrame` or not. If `true`, the menu created will have a `ScrollingFrame`; if `false`, it will have a standard `Frame` instead. Defaults to `true`.

## AddFeature
<sup>[`Yields`](## "This method will yield the current thread") [`File management`](## "This method managed files and should be handled carefully") </sup>

```lua
Comet:AddFeature(menu: string,data: table): (boolean, string)
```
Adds a feature entry to a given menu.

### Parameters

- `menu: string` - The name of the menu to add it to. This should be identical to the name provided via `AddMenu`; in other words, the name exactly as written on the button.
- `data: table` - A table of the following information;
  - `InternalName: string` - The internal name for the feature. This will be what the `Frame` for the feature is called, and how it's referenced elsewhere.
  - `Name: string` - The humanized version of the name. This is what will be listed on the UI itself.
  - `Description: string` - The text that is displayed when hovering over the name for the feature.
  - `Enabled: boolean` - A simple toggle state for the feature. Must be included even if unused.
  - `OptionType: string` - Determines the type of feature;
    - `"Toggle"` - This feature toggles on or off (uses `Enabled`).
    - `"Multi"` - This feature is a drop-down.
    - `"Press"` - This feature uses a single button press.
    - `"Text"` - This feature requires text input.
  - `MoveType: string` - Determines what Elemental Battlegrounds spell type the move is, if applicable (otherwise, `NullType`);
    - `"AoD"` - Area of Damage/Area of Effect, like Blaze Column
    - `"Blast"` - Blast, like Water Beam
    - `"Bullet"` - Bullets, like Poison Needles
    - `"Wall"` - Shields, like Spiky Shield
    - `"Heal"` - Heals, like Slime Buddies
    - `"Transport"` - Teleports, like Lightning Flash
    - `"Contact"` - Grabs, like Temporal Trap
    - `"Ultimate"` - Ultimates, like Unmatched Power of the Sun
    - `"Body"` - Buffs, like Angelic Aura
  - `Data: table` - Any data you need to pass to your feature based on the `OptionType`;
    - **`Multi`**
      - `Default: number` - The option index selected by default.
      - `Options: table` - A 1-indexed list of options
    - **`Text`**
      - `Value: string` - The text value set by the user. Setting it here sets the default value.
      - `Validation: function -> (success, errorMessage)` - A validation function to check the text's input. When called, will be provided the following arguments;
        - `input: string` - The data the user provided
        - `data: any?` - Any other information as provided by `Other`.
      - `Other: any?` - Any other information you may want to pass to your feature or its validation function.
  - `Membership: number` - A numerical number representing whether users need Comet $\color{ffffff}Basic$ (`1`), $\color{ffd700}Star$ (`2`), $\color{7fffff}Nova$ (`3`), or $\color{ff69b4}Supernova$ (`4`) to use the feature
  - `Function: function -> any?` - The function your feature will call when fired. This function will be called with the following being provided as arguments, but the parameters do not need to be defined;
    - `featureData: table` - The data provided by `Data` above.
    - `moduleData: self` - The module data for Comet.

### Example

```lua
--[[
    This creates a new Basic Multi feature (internally "MyFeature", listed as "Untitled Feature") that has two Options "Hello" and "World" (Defaulted to "Hello"). When Hello is selected, it will print "Hello" to the console; otherwise, it will print "World". It will then send a notification to the user with "Hello World!".
]]
CometUI:AddFeature("Settings",{
    InternalName = "MyFeature",
    Name = "Untitled Feature",
    Description = "A placeholder description for all your placeholder needs.",
    Enabled = false,
    OptionType = "Multi",
    MoveType = "NullType",
    Data = {
        Default = 1,
        Options = {
            [1] = "Hello",
            [2] = "World",
        },
    },
    Membership = 1,
    Function = function(featureData,moduleData)
        if featureData.Selected == 1 then
            print("Hello")
        else
            print("World")
        end
        task.spawn(moduleData.Notify,moduleData,{"Message"="Hello World!"})
    end
})
```
```lua
--[[
    This creates a new Supernova Text feature (internally "MyFeature", listed as "Untitled Feature") whose Value defaults to "Hello World". When the user changes this value, the Validation function will check for the value "forceLowerCase" in Other, then use that to determine if it checks that the input is lowercase. If forceLowerCase is true and the string is not all lowercase, it returns false with an error message; in all other cases, it returns true and nil.
]]
CometUI:AddFeature("Settings",{
    InternalName = "MyFeature",
    Name = "Untitled Feature",
    Description = "A placeholder description for all your placeholder needs.",
    Enabled = false,
    OptionType = "Text",
    MoveType = "NullType",
    Data = {
        Value = "Hello World",
        Validation = function(input, data)
            data.forceLowerCase = data.forceLowerCase or false
            if data.forceLowerCase then 
                if string.lower(input) ~= input then
                    return false, "string not lowercase when required"
                else
                    return true, nil
                end
            else return true, nil end
        end,
        Other = {forceLowerCase = true}
    },
    Membership = 4,
    Function = function(featureData,_)
        print(featureData.Data.Value)
    end
})
```