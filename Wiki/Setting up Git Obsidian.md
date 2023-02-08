[Readme guide](https://github.com/gitobsidiantutorial/obsidian-git-tut-windows/blob/main/README.md)
> [!Bible] 
> In the beginning was the Word, and the Word was with God, and the Word was God.
>The same was in the beginning with God.
>All things were made by him; and without him was not any thing made that was made.
> 
> 
  [John 1:1-3 - KJV](https://bible-api.com/John+1:1-3?translation=kjv)

![](https://www.youtube.com/watch?v=t7vkPqUj2tk)

![](https://www.youtube.com/watch?v=jNQXAC9IVRw)
## [Installation and clone a repo](https://github.com/Vinzent03/obsidian-git-mobile#installation-)
1. Create new vault
2. Change config directory in Settings -> About
3. Install Community Plugin Obsidian Git
4. Set password/personal access token and username in Settings -> Obsidian Git
5. Execute clone repo command with `https://github.com/ethanimproving/Family-Ideas.git`
6. Reload plugin

# Frequent Shortcuts

| Action                | Shortcut         |
| --------------------- | ---------------- |
| Commit Changes        | Ctrl + K         |
| Push Changes          | Ctrl + Shift + K |
| View history for file | Alt + H          |
| Format                | Shift + Alt + F  |

## Multi-caret selection / line manipulation

| Action                                    | Shortcut           |
| ----------------------------------------- | ------------------ |
| Add cursor below                          | Ctrl + Ctrl + Down |
| Add additional carrot                     | Alt + click        |
| Add the next matching text as a selection | Ctrl + D           |
| Add all matching text as selections       | Ctrl + Shift + L   |
| Copy Line Down                            | Ctrl + Alt + Down  |
| Move Line Down                            | Alt + Down         |
| [Add cursor below](https://github.com/ethanmiller1/PowerShell/blob/master/Program%20Data/Visual%20Studio%20Community/Shortcuts.md)                        |                    |
| [Column selection mode](https://forum.obsidian.md/t/column-selection-and-editing-mode/32191)                   |                    |

# Buffer size issue

[This repository currently has approximately 320 loose objects](https://stackoverflow.com/questions/21457407/git-gui-perpetually-getting-this-repository-currently-has-approximately-320-lo).

Prune
```bash
# Prune loose objects older than 2 weeks
$ git gc --prune
# Prune loose objects regardless of their age
$ git gc --prune=now
# Aggressively optimize the repository at the expense of taking much more time. The effects of this optimization are mostly persistent.
$ git gc --aggressive
```

[Git GC Docs](https://git-scm.com/docs/git-gc#_options)

Runs a number of housekeeping tasks within the current repository, such as compressing file revisions (to reduce disk space and increase performance) and removing unreachable objects which may have been created from prior invocations of git add.