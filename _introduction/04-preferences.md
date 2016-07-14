---
title: Scripting Preferences
summary: Some preferences to make developing Plugins a bit more pleasant
permalink: /introduction/preferences/
order: 400
---

If you spend non-trivial amounts of time developing Plugins for Sketch, there are some improvements you can make to your workflow using these preferences.

Since not all Sketch users are Plugin developers, it didn't make sense to give these preferences a UI in the Preferences panel. You'll need to use Terminal.app to enable / disable them.

## Define a code editor for Plugins

Have a favorite code editor? You can tell Sketch to use it to edit Plugins. For example, if you use [Atom](https://atom.io) you can do this:

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist "Plugin Editor" "/usr/local/bin/atom"
```

and relaunch Sketch, you'll see a couple of new menu items:

- Go to Preferences › Plugins and right click any of the listed Plugins. You'll see an 'Edit Code…' option that will launch your editor with the selected Plugin's code open.
- Open the Plugins menu, and you'll see an 'Edit Plugins…' option, that will launch your editor with the whole 'Plugins' folder open.

## Tweak the 'Custom Plugin…' editor

To change the font used in the 'Custom Plugin…' panel (for example, to use SF Mono) you can do this:

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont "SF Mono Light"
```

To go back to the default (Andale Mono), just delete the preference:

```shell
defaults delete ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFont
```

To change the font size for the editor (the default is 12), use

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist scriptEditorFontSize 14
```

## Listen to all actions in the Action API

<p class="warning">
  <strong>Warning:</strong> This is an extremely expensive operation, and will impact Sketch’s performance. Please use this <em>on your development system only</em> and <strong>never enable this on a customer’s computer</strong>.
</p>

When working with the new [Action API](/reference/action/) you'll probably want to listen to multiple events (specially when trying to find *which* event is the one you want to use).

To do that, use the `actionWildcardsAllowed` preference. If set to YES, scripts are allowed to register a wildcard handler for events. This is off by default, and it could have a bad effect on performance, so handle it with care.

```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist actionWildcardsAllowed -bool YES
```

Once you do that, you can tell your Plugin to call a method for every action by adding a `*` key to your `handlers.actions` object in `manifest.json`:

```json
"handlers" : {
  "actions": {
    "*": "onAction"
  }
}
```

## Always reload scripts before running

For performance reasons, Sketch caches the contents of the Plugins folder. This is very convenient for users, since Plugins run very fast, but makes your life hard if you’re a developer. That’s why we added a preference to disable this caching mechanism and force Sketch to always reload a Plugin’s code from disk:


```shell
defaults write ~/Library/Preferences/com.bohemiancoding.sketch3.plist AlwaysReloadScript -bool YES
```

If you enable this, as soon as you save your script it will be ready for testing in Sketch (bye bye relaunching it just to test a small change!)