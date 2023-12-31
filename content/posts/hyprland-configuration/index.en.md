---
title: "Hyprland Configuration"
authors: ["A7R7"]
description: "My literate config for Hyprland, a dynamic tilling window manager"
publishDate: 2023-09-15
tags: ["Hyprland", "Org-mode"]
draft: false
featuredImage: "hyprland.png"
---

<!--more-->


## Intro {#intro}

This is my hyprland literate configuration.
Codes from hyprland.conf, pyprland.json, etc are generated from this org file,
by running `M-x org-babel-tangle` inside emacs.

You may see comments like this in hyprland.conf, if you do not use emacs org mode, just ignore them, they are used by emacs `M-x org-babel-detangle`.

```example
# [[file:hyprland.org::*Input][Input:1]]
# Input:1 ends here
```


## Variables {#variables}

Variables are "options" of hyprland. Each variable has a unique value assigned to it.
Basically, variables controls how your hyprland desktop looks like.


### Input {#input}

Keyboard, mouse and touch pad input settings.

```cfg
input {
  kb_layout = us
  kb_variant =
  kb_model =
  kb_options = caps:escape,shift:both_capslock
  kb_rules =

  follow_mouse = 1

  touchpad {
    natural_scroll = true
  }
  sensitivity = 1 # -1.0 - 1.0, 0 means no modification.
  repeat_rate = 28
  repeat_delay = 400
}
```


### General {#general}

See [Configuring/Variables](https://wiki.hyprland.org/Configuring/Variables/) for more

```cfg
general {
  gaps_in = 10
  gaps_out = 20
  border_size = 4
  col.active_border = rgba(33ccffee) rgba(00ff99ee) 45deg
  col.inactive_border = rgba(24283be6)

  layout = dwindle
# layout = master
}
```


### Decoration {#decoration}

Configureing window decorations, including opacity, border rounding, shadow, etc.
See [Configuring/Variables](https://wiki.hyprland.org/Configuring/Variables/) for more

```cfg
decoration {
  active_opacity=1.0
  fullscreen_opacity=1.0
  rounding = 11
  multisample_edges=true
  drop_shadow = true
  shadow_range = 4
  shadow_render_power = 3
  col.shadow = rgba(1a1a1aee)
 # blur = false; # deprecated
  blur {
    # enable kawase window background blur. default true
    enabled = false
    # blur size (distance). default 8
    size = 8
    # the amount of passes to perform. default 1
    passes = 1
    # make the blur layer ignore the opacity of the window.
    # default false
    ignore_opacity = false
    # whether to enable further optimizations to the blur.
    # massively improve performance. default true
    new_optimizations = true
    # make floating windows ignore tiled windows in their blur.
    # Only available if blur_new_optimizations is true.
    # Will reduce overhead on loating blur significantly.
    xray = true
    # how much noise to apply. 0.0 - 1.0. default 0.0117
    noise = 0.0117
    # contrast modulation for blur. 0.0 - 2.0. default 0.8916
    contrast = 0.8916
    # brightness  modulation for blur. 0.0 - 2.0. default 0.8172
    brightness = 0.8172
   }
 }
```


### Animations {#animations}

See <https://wiki.hyprland.org/Configuring/Animations/> for more

```cfg
animations {
    enabled = true
    bezier = myBezier, 0.05, 0.9, 0.1, 1.05
    animation = windows, 1, 3, default
    animation = windowsOut, 1, 4, default, popin 50%
    animation = border, 1, 5, default
    animation = borderangle, 1, 5, default
    animation = fade, 1, 5, default
    animation = workspaces, 1, 2, default
    animation = specialWorkspace, 1, 2.5, default, slidevert
    # animation = specialWorkspace, 1, 3, default, fade
}
```


### Layout {#layout}

Configuring how the windows are arranged when multiple window are tiled together in one workspace. Note that the variable [general.layout](#general) controls the global layout. You can alse set workspace rules to control per workspace layout.

[Dwindle-Layout](https://wiki.hyprland.org/Configuring/Dwindle-Layout/)

```cfg
dwindle {
  pseudotile = true # master switch for pseudotiling. Enabling is bound to mainMod + P in the keybinds section below
  preserve_split = true # you probably want this
}
```

[Master-Layout](https://wiki.hyprland.org/Configuring/Master-Layout/)

```cfg
master {
  new_is_master = true
}
```


### Gestures {#gestures}

Configuring touchpad actions.
See <https://wiki.hyprland.org/Configuring/Variables/> for more

```cfg
gestures {
  workspace_swipe = true
  workspace_swipe_fingers = 3;
  workspace_swipe_distance = 2500;
}
```


### Devices {#devices}

See <https://wiki.hyprland.org/Configuring/Keywords/#executing> for more

```cfg
device:epic mouse V1 {
  sensitivity = -0.5
}
```


### Misc {#misc}

```cfg
misc {
  disable_hyprland_logo = true
    disable_splash_rendering = true
    vrr = 2
}
```


## Monitors {#monitors}

My monitor information are secrets. Therefore I put it inside monitor.conf and git-ignored it.

```cfg
source=~/.config/hypr/monitor.conf
```


## Executes {#executes}

Execute a shell script on startup of the compositor or on each time it's reloaded.

`exec-once=command`
: will execute only on launch

`exec=command`
: will execute on each reload

<!--listend-->

```cfg
# exec-once = dbus-update-activation-environment --all
exec-once = /usr/bin/gnome-keyring-daemon --start --components=secrets
exec-once = /usr/lib/polkit-gnome/polkit-gnome-authentication-agent-1 &
exec-once = dunst &
exec-once = fcitx5 -d
exec-once = pulseaudio -D

exec-once = wl-paste --type text --watch cliphist store #Stores only text data
exec-once = wl-paste --type image --watch cliphist store #Stores only image data

# exec-once = clash-verge;
# exec-once = pkill eww; eww daemon; eww open-many bar0 bar1 bar2;
exec-once = ags -b hypr
exec-once = pkill hyprpaper; hyprpaper;

# exec-once = bash -c ~/.config/hypr/bin/init.sh

# emacs client has bugs
# exec-once = emacs --init-directory=~/.doomemacs.d --daemon
```


## Keybinds {#keybinds}


#### Helpful variables {#helpful-variables}

```cfg
# helpful variables
$activeMonitorId="$(hyprctl -j monitors | jq -r '.[] | select(.focused == true) | .id')"
$activeWorkspaceId="$(hyprctl -j activeworkspace | jq -r '.id')"
$focusWorkspace="hyprctl dispatch workspace"
$focusMonitor="hyprctl dispatch focusmonitor"
$move2Workspace="hyprctl dispatch movetoworkspace"
$specialWorkspaceId="$(hyprctl -j activewindow | jq -r '.workspace.name' | cut -d':' -f2)"
$toggleOverview=
$mainMod = SUPER

```


#### Launch applications {#launch-applications}

```cfg
# applications
bind = $mainMod, Return, exec, kitty --single-instance
bind = $mainMod, E, exec, thunar
bind = $mainMod, B, exec, vivaldi-stable --enable-features=UseOzonePlatform --ozone-platform=wayland --enable-wayland-ime %U

bind = $mainMod, N, exec, neovide --multigrid
bind = $mainMod, M, exec, emacs

# no longer use rofi to launch apps, but ags's internal launcher
# bind = $mainMod, R, exec, ~/.config/rofi/launcher.sh
bind = $mainMod, F, exec, ~/.config/rofi/file.sh
bind = $mainMod, V, exec, ~/.config/rofi/clipboard.sh

```

-   Grimblast

<!--listend-->

```cfg
bind=,Print,execr, grimblast --notify --cursor copysave area $(xdg-user-dir PICTURES)/$(date +'%Y-%m-%d-%H-%M-%S_1.png')
bind=SUPER,Print,exec,grimblast --notify save output $(xdg-user-dir PICTURES)/Screenshots/$(date +'%Y%m%d%H%M%S_1.png')
bind=SUPERSHIFT,Print,exec,grimblast save output - | swappy -f -
```


#### Window Functions {#window-functions}

```cfg
#function
bind = $mainMod , Q, killactive,
bind = $mainMod , S, togglesplit, # dwindle
bind = $mainMod , G, togglegroup,
# bind = $mainMod , O, execr, ~/.config/hypr/bin/eww_toggle_overview.sh
bind = $mainMod ALT, F9,  pseudo, # dwindle
bind = $mainMod ALT, F10, togglefloating,
bind = $mainMod ALT, F11, fullscreen, 0
```


#### Desktop Functions {#desktop-functions}

```cfg
# bind = $mainMod ALT, Delete, exec, wlogout
bind = $mainMod CTRL ALT, Delete, exec, kill
bindle = , XF86AudioRaiseVolume,    exec, pactl set-sink-volume @DEFAULT_SINK@ +1%
bindle = , XF86AudioLowerVolume,    exec, pactl set-sink-volume @DEFAULT_SINK@ -1%
bindle = , XF86MonBrightnessUp,     exec, brightnessctl set 5%+ -q
bindle = , XF86MonBrightnessDown,   exec, brightnessctl set 5%- -q
bindle = , XF86KbdBrightnessUp,     exec, bash ~/.config/eww/scripts/brightness kbd up
bindle = , XF86KbdBrightnessDown,   exec, bash ~/.config/eww/scripts/brightness kbd down
bindl  = , XF86AudioStop,           exec, playerctl stop
bindl  = , XF86AudioPause,          exec, playerctl pause
bindl  = , XF86AudioPrev,           exec, playerctl previous
bindl  = , XF86AudioNext,           exec, playerctl next
bindl  = , XF86AudioPlay,           exec, playerctl play-pause

bind = CTRL ALT, F1, exec, notify-send "CTRL ALT F1"
bind = CTRL ALT, F2, exec, notify-send "CTRL ALT F2"
```

```cfg
# AGS
bind = $mainMod ALT,  Delete, exec, ags -b hypr toggle-window powermenu
bind = CTRL SHIFT, R,   exec, ags -b hypr quit; ags -b hypr
bind = SUPER, R,        exec, ags -b hypr toggle-window applauncher
bind = , XF86Launch4,   exec, ags -b hypr -r "ags.Service.Recorder.start()"
bind = , XF86PowerOff,  exec, ags -b hypr toggle-window powermenu
bind = SUPER, Tab,      exec, ags -b hypr toggle-window overview
# bind  = , XF86Launch1, exec,

# Laptop
bindle = , XF86MonBrightnessUp,     exec, ags -b hypr -r "ags.Service.Brightness.screen += 0.05; ags.Service.Indicator.display()"
bindle = , XF86MonBrightnessDown,   exec, ags -b hypr -r "ags.Service.Brightness.screen -= 0.05; ags.Service.Indicator.display()"
bindle = , XF86KbdBrightnessUp,     exec, ags -b hypr -r "ags.Service.Brightness.kbd++; ags.Service.Indicator.kbd()"
bindle = , XF86KbdBrightnessDown,   exec, ags -b hypr -r "ags.Service.Brightness.kbd--; ags.Service.Indicator.kbd()"
bindle = , XF86AudioRaiseVolume,    exec, ags -b hypr -r "ags.Service.Audio.speaker.volume += 0.05; ags.Service.Indicator.speaker()"
bindle = , XF86AudioLowerVolume,    exec, ags -b hypr -r "ags.Service.Audio.speaker.volume -= 0.05; ags.Service.Indicator.speaker()"
bindl  = , XF86AudioPlay,           exec, ags -b hypr -r "ags.Service.Mpris.players.pop()?.playPause()"
bindl  = , XF86AudioStop,           exec, ags -b hypr -r "ags.Service.Mpris.players.pop()?.stop()"
bindl  = , XF86AudioPause,          exec, ags -b hypr -r "ags.Service.Mpris.players.pop()?.pause()"
bindl  = , XF86AudioPrev,           exec, ags -b hypr -r "ags.Service.Mpris.players.pop()?.previous()"
bindl  = , XF86AudioNext,           exec, ags -b hypr -r "ags.Service.Mpris.players.pop()?.next()"
bindl  = , XF86AudioMicMute,        exec, pactl set-source-mute @DEFAULT_SOURCE@ toggle

# Print
bind = ,Print, exec, ags -b hypr run-js "ags.Service.Recorder.screenshot()"
bind = SHIFT,Print, exec, ags -b hypr run-js "ags.Service.Recorder.screenshot(true)"


```


#### Move Focus {#move-focus}

```cfg
bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d
bind = $mainMod, H, movefocus, l
bind = $mainMod, J, movefocus, d
bind = $mainMod, K, movefocus, u
bind = $mainMod, L, movefocus, r

bind = $mainMod, 1, execr, "$focusWorkspace $activeMonitorId"1
bind = $mainMod, 2, execr, "$focusWorkspace $activeMonitorId"2
bind = $mainMod, 3, execr, "$focusWorkspace $activeMonitorId"3
bind = $mainMod, 4, execr, "$focusWorkspace $activeMonitorId"4
bind = $mainMod, 5, execr, "$focusWorkspace $activeMonitorId"5
bind = $mainMod, 6, execr, "$focusWorkspace $activeMonitorId"6
bind = $mainMod, 7, execr, "$focusWorkspace $activeMonitorId"7
bind = $mainMod, 8, execr, "$focusWorkspace $activeMonitorId"8
bind = $mainMod, 9, execr, "$focusWorkspace $activeMonitorId"9
bind = $mainMod, 0, execr, "$focusWorkspace $((1+$activeMonitorId))"0

#    Move focuse inside focusing monitor
# bind = $mainMod ALT, H, execr, "$focusWorkspace" "$activeMonitorId""$(((activeWorkspaceId-1)%10))"
# bind = $mainMod ALT, L, execr, "$focusWorkspace" "$activeMonitorId""$(((activeWorkspaceId+1)%10))"
bind = $mainMod , COMMA,       execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 1 ]; then "$focusWorkspace $(($activeWorkspaceId+9))"; else "$focusWorkspace $(($activeWorkspaceId-1))" ;fi`
bind = $mainMod , PERIOD,      execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 0 ]; then "$focusWorkspace $(($activeWorkspaceId-9))"; else "$focusWorkspace $(($activeWorkspaceId+1))" ;fi`
bind = $mainMod , BracketLeft, execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 1 ]; then "$focusWorkspace $(($activeWorkspaceId+9))"; else "$focusWorkspace $(($activeWorkspaceId-1))" ;fi`
bind = $mainMod , BracketRight,execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 0 ]; then "$focusWorkspace $(($activeWorkspaceId-9))"; else "$focusWorkspace $(($activeWorkspaceId+1))" ;fi`
bind = $mainMod SHIFT, COMMA,  execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 1 ]; then "$move2Workspace $(($activeWorkspaceId+9))"; else "$move2Workspace $(($activeWorkspaceId-1))" ;fi`
bind = $mainMod SHIFT, PERIOD, execr, `if [ $(("$activeWorkspaceId" % 10)) -eq 0 ]; then "$move2Workspace $(($activeWorkspaceId-9))"; else "$move2Workspace $(($activeWorkspaceId+1))" ;fi`
bind = $mainMod ALT, h, workspace, m-1
bind = $mainMod ALT, l, workspace, m+1

bind = $mainMod , Tab, workspace, previous
# bind = $mainMod , COMMA,  workspace, m-1
# bind = $mainMod , PERIOD, workspace, m+1
```


#### Move Window {#move-window}

```cfg
#  Move window{{{2
#    Move window to direction{{{
bind = $mainMod SHIFT, left, movewindow, l
bind = $mainMod SHIFT, right, movewindow, r
bind = $mainMod SHIFT, up, movewindow, u
bind = $mainMod SHIFT, down, movewindow, d
bind = $mainMod SHIFT, H, movewindow, l
bind = $mainMod SHIFT, J, movewindow, d
bind = $mainMod SHIFT, K, movewindow, u
bind = $mainMod SHIFT, L, movewindow, r
#}}}
```

```cfg
#    Move window to workspace {{{
bind = $mainMod SHIFT, 1, execr, "$move2Workspace" "$activeMonitorId"1
bind = $mainMod SHIFT, 2, execr, "$move2Workspace" "$activeMonitorId"2
bind = $mainMod SHIFT, 3, execr, "$move2Workspace" "$activeMonitorId"3
bind = $mainMod SHIFT, 4, execr, "$move2Workspace" "$activeMonitorId"4
bind = $mainMod SHIFT, 5, execr, "$move2Workspace" "$activeMonitorId"5
bind = $mainMod SHIFT, 6, execr, "$move2Workspace" "$activeMonitorId"6
bind = $mainMod SHIFT, 7, execr, "$move2Workspace" "$activeMonitorId"7
bind = $mainMod SHIFT, 8, execr, "$move2Workspace" "$activeMonitorId"8
bind = $mainMod SHIFT, 9, execr, "$move2Workspace" "$activeMonitorId"9
bind = $mainMod SHIFT, 0, execr, "$move2Workspace" "$((1+$activeMonitorId))"0
#}}}
```

```cfg
#    Move window to monitor {{{
# bind = $mainMod SHIFT, F1, movewindow, mon:$screen1
# bind = $mainMod SHIFT, F2, movewindow, mon:$screen2
# bind = $mainMod SHIFT, F3, movewindow, mon:$screen3
#}}}
```

```cfg

#    Move window to special workspace {{{
bind = $mainMod SHIFT, S,   movetoworkspace, special
# bind = $mainMod SHIFT, F1,  movetoworkspace, special:1
bind = $mainMod SHIFT, F2,  movetoworkspace, special:2
bind = $mainMod SHIFT, F3,  movetoworkspace, special:3
bind = $mainMod SHIFT, F4,  movetoworkspace, special:4
bind = $mainMod SHIFT, F5,  movetoworkspace, special:5
bind = $mainMod SHIFT, F6,  movetoworkspace, special:6
bind = $mainMod SHIFT, F7,  movetoworkspace, special:7
bind = $mainMod SHIFT, F8,  movetoworkspace, special:8
bind = $mainMod SHIFT, F9,  movetoworkspace, special:9
bind = $mainMod SHIFT, F10, movetoworkspace, special:10
bind = $mainMod SHIFT, F11, movetoworkspace, special:11
bind = $mainMod SHIFT, F12, movetoworkspace, special:12
#}}}
```

```cfg

#    Move float window position{{{
binde = $mainMod ALT, left,moveactive,-50 0
binde = $mainMod ALT, down,moveactive, 0 50
binde = $mainMod ALT, up,moveactive, 0 -50
binde = $mainMod ALT, right,moveactive, 50 0
binde = $mainMod ALT, H,moveactive,-50 0
binde = $mainMod ALT, J,moveactive, 0 50
binde = $mainMod ALT, K,moveactive, 0 -50
binde = $mainMod ALT, L,moveactive, 50 0
#}}}
#
#}}}2
```

-   Mouse actions to move window, resize window and swap workspaces.

<!--listend-->

```cfg
#  Mouse window action{{{
bindm= $mainMod, mouse:272, movewindow
bindm= $mainMod, mouse:273, resizewindow
bind = $mainMod, mouse_down, workspace, e+1
bind = $mainMod, mouse_up, workspace, e-1
#}}}
```


#### Special workspace {#special-workspace}

```cfg
#  Special workspace{{{
#  hide a showing specialWorkspace
bind = $mainMod, escape, execr, hyprctl dispatch togglespecialworkspace $specialWorkspaceId
# bind = $mainMod, F1,  togglespecialworkspace, 1
# bind = $mainMod, F2,  togglespecialworkspace, 2
# bind = $mainMod, F3,  togglespecialworkspace, 3
bind = $mainMod, F4,  togglespecialworkspace, 4
bind = $mainMod, F5,  togglespecialworkspace, 5
bind = $mainMod, F6,  togglespecialworkspace, 6
bind = $mainMod, F7,  togglespecialworkspace, 7
bind = $mainMod, F8,  togglespecialworkspace, 8
bind = $mainMod, F9,  togglespecialworkspace, 9
bind = $mainMod, F10, togglespecialworkspace, 10
bind = $mainMod, F11, togglespecialworkspace, 11
bind = $mainMod, F12, togglespecialworkspace, 12
#}}}

#}}}1
```


#### Window resize {#window-resize}

```cfg
binde = $mainMod CTRL, left,resizeactive,-50 0
binde = $mainMod CTRL, down,resizeactive, 0 50
binde = $mainMod CTRL, up,resizeactive, 0 -50
binde = $mainMod CTRL, right,resizeactive, 50 0
binde = $mainMod CTRL, H,resizeactive,-50 0
binde = $mainMod CTRL, J,resizeactive, 0 50
binde = $mainMod CTRL, K,resizeactive, 0 -50
binde = $mainMod CTRL, L,resizeactive, 50 0

bind  = $mainMod CTRL, R, submap, resize
submap = resize
binde = , left , resizeactive,-50 0
binde = , down , resizeactive, 0 50
binde = , up   , resizeactive, 0 -50
binde = , right, resizeactive, 50 0
binde = , h    , resizeactive,-50 0
binde = , j    , resizeactive, 0 50
binde = , k    , resizeactive, 0 -50
binde = , l    , resizeactive, 50 0
bind  = ,escape, submap, reset
bind  = $mainMod SHIFT, R, submap, reset
submap = reset
```


## Window rules {#window-rules}


##### Floats {#floats}

These are the windows I want to make float.

```cfg
windowrule = float, ^(xdg-desktop-portal)$
windowrule = float, ^(xdg-desktop-portal-gnome)$
windowrule = float, ^(Rofi)$
windowrule = float, ^(wlogout)$
windowrule = float, ^(org.gnome.Calculator)$
windowrule = float, ^(org.gnome.Settings)$
windowrule = float, ^(org.gnome.design.Palette)$
windowrule = float, ^(eww)$
windowrule = float, ^(pavucontrol)$
windowrule = float, ^(nm-connection-editor)$
windowrule = float, ^(blueberry.py)$
windowrulev2 = float, class:^(blueman-manager)$, title: ^(Bluetooth Devices)$

windowrule = float, ^(Color Picker)$
windowrule = float, ^(Network)$
windowrule = float, ^(xdg-desktop-portal)$
windowrule = float, ^(xdg-desktop-portal-gnome)$
windowrule = float, ^(transmission-gtk)$
windowrule = float, ^(hmcl)$
windowrulev2 = float, class:^(thunar)$,title:^(?!.* - Thunar$).*$
windowrule = float, ^(org.fcitx.fcitx5-config-qt)
windowrule = float, ^(file-roller)$
```


##### X Menus {#x-menus}

Some xwayland window's menu have a dim black border by default. However hyprland applys a corner rounding to it by cutting off its 4 corners, making the corner having no border, which looks strange.

These menus have no class (class=""). So the following rule can disable the corner rounding for them.

```cfg
windowrulev2 = rounding 0, class:^()$, floating:1, xwayland:1
windowrulev2 = rounding 0, class:^(GoldenDict-ng)$, floating:1, xwayland:1

```


##### VLC {#vlc}

```cfg
windowrulev2 = float, class:^(vlc)$,title:^(Adjustments and Effects — VLC media player)$
windowrulev2 = float, class:^(vlc)$,title:^(Simple Preferences — VLC media player)$
```


##### Emacs {#emacs}

Ediff

```cfg
windowrulev2 = float, class:^(Emacs)$,title:^(Ediff)$
windowrulev2 = noborder, class:^(Emacs)$,title:^(Ediff)$
```

Minibuf

```cfg
windowrulev2 = float, class:^(emacs)$,title:^( \*Minibuf-\d+\*)$
windowrulev2 = noborder, class:^(emacs)$,title:^( \*Minibuf-\d+\*)$
```

Eaf

```cfg
windowrule = float, class:^(python3)$, title:^(eaf.py)$
windowrule = noanim, class:^(python3)$, title:^(eaf.py)$
windowrule = nofocus, class:^(python3)$, title:^(eaf.py)$
```

Holo-layer

```cfg
windowrulev2 = float, class:^(python3)$, title:^(holo_layer.py)$
windowrulev2 = nofocus, class:^(python3)$, title:^(holo_layer.py)$
windowrulev2 = noanim, class:^(python3)$, title:^(holo_layer.py)$
```


##### Steam {#steam}

Steam has a friend list window. By default when opening friends list, it will be tiled together with steam, which isn't nice. Adding this rule makes Friends list float.

```cfg
windowrulev2 = float, class:^(steam)$, title:^(Friends List)
```


##### Bitwig Studio {#bitwig-studio}

Bigwig Studio is a music production studio. It has buttons that are dragable. When dragging those buttons, a tiny tooltip window will float above the button showing its current value.

However, on hyprland, when dragging those buttons, this tooltip window will be auto focused, which then lead to bitwig's window losing its focus, and the drag action failing to be recognized. Thus the button appears to be undragable.

The tooltip window's class is "", and it's floating. Therefore adding the following rule fixed this issue. From my experience so far, this do not break anything else.

```cfg
windowrulev2 = noinitialfocus, class:^()$, floating:1
```


## Workspace Rules {#workspace-rules}

Currently I have no workspace rules.


## Pyprland {#pyprland}

[Pyprland](https://github.com/hyprland-community/pyprland) is not part of hyprland. It is a community driven program that hosts process for multiple Hyprland plugins.
It is a program, therefore you don't need to install it the plugin way.

Below are configurations for pyprland. All the json codes are tangled to `pyprland.json`, and other codes are tangled to `hyprland.conf`


### Core {#core}

First let's launch pyprland on startup.

```cfg
exec-once = pypr
```

Pyprland's main config goes to `~/.config/hypr/pyprland.json`

```json
{
  "pyprland": {
    "plugins": [
      "scratchpads",
      "lost_windows"
    ]
  },
  "scratchpads": {
    "term": {
      "command": "kitty --class kitty-dropterm",
      "animation": "fromTop",
      "lazy": true
    },
    "files": {
      "command": "nautilus",
      "animation": "fromTop",
      "lazy": true
    },
    "dict": {
      "command": "goldendict",
      "animation": "fromTop",
      "lazy": true
    },
    "tty-clock": {
      "command": "kitty --class kitty-tty-clock tty-clock -cs",
      "animation": "fromTop",
      "lazy": true
    },
    "cava": {
      "command": "kitty --class kitty-cava cava",
      "animation": "fromBottom",
      "lazy": true
    },
    "pipes": {
      "command": "kitty --class kitty-pipes pipes",
      "animation": "fromLeft",
      "lazy": true
    },
    "cmatrix": {
      "command": "kitty --class kitty-cmatrix cmatrix",
      "animation": "fromRight",
      "lazy": true
    },
    "music": {
      "command": "tauon",
      "animation": "fromTop",
      "unfocus": "hide",
      "lazy": true
    },
    "volume": {
      "command": "pavucontrol",
      "animation": "fromRight",
      "unfocus": "hide",
      "lazy": true
    },
    "network": {
      "command": "nm-connection-editor",
      "animation": "fromRight",
      "lazy": true
    },
    "bluetooth": {
      "command": "blueman-manager",
      "animation": "fromLeft",
      "lazy": true
    },
    "clash": {
      "command": "clash-verge",
      "animation": "fromTop",
      "lazy": false
    },
    "placeholder": {
      "command": "",
      "lazy": true
    }
  }
}
```


### Scratchpads {#scratchpads}

Scratchpads is one plugin hosted by pyprland.

A scrachpad acts like a window in a special workspace, and can be toggled out by a command (`pypr toggle xxx`) or a key if bind this command to a key.

However, they are different to special workspaces in several ways:

1.  a scratch pad always opens a certain window (that you declared) when toggled out.
2.  The window is not inside a special workspace. It's just a floating window with predefined size and location on the screen.
3.  It can toggled out from 4 directions (fromTop, fromBottom, fromLeft, fromRight)
4.  See the [scratchpads section of pyprlad wiki](https://github.com/hyprland-community/pyprland/wiki/Plugins#scratchpads) for more

All the following json codes are tangled under the  "`scratchpads`" variable in `pyprland.json`


#### Dropterm {#dropterm}

```json
"term": {
  "command": "kitty --class kitty-dropterm",
  "animation": "fromTop",
  "lazy": true
},
```

```cfg
bind = $mainMod, F1, exec, pypr toggle term
$dropterm  = ^(kitty-dropterm)$
windowrule = float,$dropterm
windowrule = workspace special silent,$dropterm
windowrule = size 75% 60%,$dropterm
```


#### File-manager {#file-manager}

```json
"files": {
  "command": "nautilus",
  "animation": "fromTop",
  "lazy": true
},
```

```cfg
bind = $mainMod, F2, exec, pypr toggle files
windowrule = float, ^(org.gnome.Nautilus)$
windowrule = workspace special silent,^(org.gnome.Nautilus)$
windowrule = size 75% 60%,^(org.gnome.Nautilus)$
```


#### Dict {#dict}

```json
"dict": {
  "command": "goldendict",
  "animation": "fromTop",
  "lazy": true
},
```

```cfg
bind = $mainMod, F3, exec, pypr toggle dict
windowrulev2 = float, class:^(GoldenDict)$
windowrulev2 = workspace special silent, class:^(GoldenDict)$
windowrulev2 = size 75% 60%, class:^(GoldenDict)$
```


#### Fancy-Terms {#fancy-terms}

```cfg
bind = $mainMod, F4, exec, pypr toggle pipes; sleep 0.07; pypr toggle cava;
bind = $mainMod, F4, exec, pypr toggle cmatrix; sleep 0.07; pypr toggle tty-clock
```

-   tty-clock

<!--listend-->

```json
"tty-clock": {
  "command": "kitty --class kitty-tty-clock tty-clock -cs",
  "animation": "fromTop",
  "lazy": true
},
```

```cfg
windowrule = float, ^(kitty-tty-clock)$
windowrule = workspace special silent, ^(kitty-tty-clock)$
windowrule = size 40% 45%, ^(kitty-tty-clock)$
```

-   cava

<!--listend-->

```json
"cava": {
  "command": "kitty --class kitty-cava cava",
  "animation": "fromBottom",
  "lazy": true
},
```

```cfg
windowrule = float, ^(kitty-cava)$
windowrule = workspace special silent, ^(kitty-cava)$
windowrule = size 40% 45%, ^(kitty-cava)$
```

-   Pipes

<!--listend-->

```json
"pipes": {
  "command": "kitty --class kitty-pipes pipes",
  "animation": "fromLeft",
  "lazy": true
},
```

```cfg
windowrule = float, ^(kitty-pipes)$
windowrule = workspace special silent, ^(kitty-pipes)$
windowrule = size 25% 60%, ^(kitty-pipes)$
```

-   CMatrix

<!--listend-->

```json
"cmatrix": {
  "command": "kitty --class kitty-cmatrix cmatrix",
  "animation": "fromRight",
  "lazy": true
},
```

```cfg
windowrule = float, ^(kitty-cmatrix)$
windowrule = workspace special silent, ^(kitty-cmatrix)$
windowrule = size 25% 60%, ^(kitty-cmatrix)$
```


#### Music {#music}

Toggle tauon music box from top of the screen.

```json
"music": {
  "command": "tauon",
  "animation": "fromTop",
  "unfocus": "hide",
  "lazy": true
},
```

```cfg
bind = $mainMod, F5, exec, pypr toggle music
windowrule = float,^(tauonmb)$
windowrule = workspace special,^(tauonmb)$
windowrule = size 50% 50%,^(tauonmb)$
```


#### Pavucontrol {#pavucontrol}

Toggle pavucontrol from right of the screen.

```json
"volume": {
  "command": "pavucontrol",
  "animation": "fromRight",
  "unfocus": "hide",
  "lazy": true
},
```

```cfg
bind = $mainMod, F5, exec, pypr toggle volume
windowrule = float,^(pavucontrol)$
windowrule = workspace special silent,^(pavucontrol)$
windowrule = size 25% 25%,^(pavucontrol)$
```


#### Network {#network}

Toggle network-manager from right, bluetooth-manager from left,
and clash-verge from top.

-   Network-manager

<!--listend-->

```json
"network": {
  "command": "nm-connection-editor",
  "animation": "fromRight",
  "lazy": true
},
```

```cfg
bind = $mainMod, F9, exec, pypr toggle network
windowrule = float, ^(nm-connection-editor)$
windowrulev2 = workspace special silent, class:^(nm-connection-editor)$, title:^(Network Connections)$
windowrule = size 18% 40%,^(nm-connection-editor)$
```

-   Bluetooth

<!--listend-->

```json
"bluetooth": {
  "command": "blueman-manager",
  "animation": "fromLeft",
  "lazy": true
},
```

```cfg
bind = $mainMod, F9, exec, pypr toggle bluetooth
windowrule = float, ^(blueman-manager)
windowrule = workspace special silent, ^(blueman-manager)$
windowrule = size 18% 40%,^(blueman-manager)$
```

-   Clash-Verge

<!--listend-->

```json
"clash": {
  "command": "clash-verge",
  "animation": "fromTop",
  "lazy": false
},
```

```cfg
bind = $mainMod, F9, exec, pypr toggle clash
windowrule = float, ^(clash-verge)$
windowrule = workspace special silent, ^(clash-verge)$
windowrule = size 50% 50%,^(clash-verge)$
```


### Lost Windows {#lost-windows}

```nil

```
