##Permissions used:

#device=all

This is required in order for arbitrary gamepads to work, as well as gpu acceleration.

If you don't like this and don't want gamepad support it is possible to change this to ```device=dri``` so that opengl will still work.

#filesystem=host:ro

Grants read-only access to the host file system, Dolphin requires this in order to display the contents of your games directory on the main window.

You can disable this and dolphin will still work, but you'll need to run games manually through the File->Open menu each time.

#socket=pulseaudio

Dolphin needs this to play audio.

#env=QT_QPA_PLATFORM=xcb

This env variable fixes a hypothetical case that could prevent Dolphin from running if it were set to wayland, it's unlikely but we keep it just in case.

Usually this would only happen if the user had globally set the variable in order to force applications to run on wayland, otherwise it is safe to drop.

#socket=x11

Necessary in order to display the window, Dolphin has no wayland support at this point.

#share=network

It's required by some features to work, such as netplay, firmware updates and the optional telemetry.

It's safe to disable if you don't want those features.

#share=ipc

Graphical applications will run slowly without this.

#allow=bluetooth

Only necessary when using "Real Wii remotes" in conjunction with the feature labeled "Emulate the Wii's Bluetooth adapter".

It's safe to disable if that feature is not in use, generic bluetooth gamepads will still work without it. Actual Wii remotes can also work without this option when using the separate passthrough feature.

#filesystem=xdg-run/app/com.discordapp.Discord:create

Necessary for discord (a nonfree messaging service) integration.

Can be safely dropped if discord is not used.

#talk-name=org.freedesktop.ScreenSaver

Required for screensaver inhibition during gameplay.

It can be disabled but your screensaver might trigger during gameplay depending on your input device and screensaver configuration.
