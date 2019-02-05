---
layout: post
title: Theming My Tools Per Project
tags:
  - iterm2
  - vscode
  - theme
  - mac
---

Theming the tools I use everyday helps me move efficiently between projects' windows and applications. I can tell what project a particular instance of VSCode is without needing to read a word.

In this post, I will detail step by step instructions to theme VSCode, Mac Folder Icons, and Slack. If there are any tools you use not listed here, [let me know](https://twitter.com/attackrunryan).


# VSCode per-workspace color overrides
I use the product's branding for color choices. Here's the settings I use. You could take this much further, but I mainly use the `titleBar` and `statusBar`.

![VSCode Workspace Theming](/images/vscode-theming.png)

1. Create a project workspace
  - `File > Save Workspace As...` 
  - `project.code-workspace`

2. Override colors. Check the [official documentation](https://code.visualstudio.com/api/references/theme-color) for a list of all the possible overrides.

Here's my `<project>.code-workspace`
```
{
  "folders": [
    {
      "path": "."
    }
  ],
  "settings": {
    "workbench.colorCustomizations": {
      "sideBar.background": "#202021",
      "editor.background": "#202021",
      "titleBar.border": "#5B26AF",
      "titleBar.activeForeground": "#E450AE"
    },
  }
}
```

# Custom Folder Icons

End result:
![Folder Icons](/images/customFolderIcon/07_result.png)

I recently started a new project, [Lets Go Design](https://lets-go.design). Naturally, this means a new top level folder in my `_projects/` directory. I like to customize the icons of these folders for visual cues when browsing these folders.

I've created the directory, but it still has the default folder icon.

![Folder Icons](/images/customFolderIcon/01_current.png)

1. Find the image file you want to use as the custom icon and open it in Preview. `CMD + C` to copy the image to your clipboard.

    ![Preview Copy](/images/customFolderIcon/02_previewCopy.png)

2. Right click on the folder you want to customize, and click "Get Info"

    ![Get Info](/images/customFolderIcon/03_getInfo.png)

3. In the top right of the info window, select the folder icon.

    Unselected
    ![Top](/images/customFolderIcon/04_top.png)

    Selected
    ![Top Selected](/images/customFolderIcon/05_topSelected.png)

4. `CMD + V` to paste the image.

    ![Paste](/images/customFolderIcon/06_paste.png)

5. That's it!

    ![Result](/images/customFolderIcon/07_result.png)

# Slack Themes

Slack comes with a number of preset theme options, but you can override these themes to your liking.

1. Navigate to your workspace's preferences.

    ![Preferences](/images/slackTheme/01_preferences.png)

2. Select "Sidebar" and scroll all the way down to the bottom. If you don't see the Custom Theme option, you may need to click advanced options.

    ![Sidebar](/images/slackTheme/02_sidebar.png)

3. I tend to set "Active Item" to the brand's primary color.

    ![Theme](/images/slackTheme/03_theme.png)
