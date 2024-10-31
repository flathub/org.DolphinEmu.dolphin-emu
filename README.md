# README

## Table of Contents

- [How to connect your Wii remotes](#how-to-connect-your-wii-remotes)
- [Passthrough a Bluetooth adapter](#passthrough-a-bluetooth-adapter)
- [Emulate the Wii's Bluetooth adapter](#emulate-the-wiis-bluetooth-adapter)
- [How to enable motion controls on non-Wii controllers](#how-to-enable-motion-controls-on-non-wii-controllers)
- [Permissions Used](#permissions-used)
    - [device=all](#deviceall)
    - [filesystem=host:ro](#filesystemhostro)
    - [socket=pulseaudio](#socketpulseaudio)
    - [env=QT_QPA_PLATFORM=xcb](#envqt_qpa_platformxcb)
    - [socket=x11](#socketx11)
    - [share=network](#sharenetwork)
    - [share=ipc](#shareipc)
    - [allow=bluetooth](#allowbluetooth)
    - [filesystem=xdg-run/app/com.discordapp.Discord:create](#filesystemxdg-runappcomdiscordappdiscordcreate)
    - [talk-name=org.freedesktop.ScreenSaver](#talk-nameorgfreedesktopscreensaver)
- [dolphin-tool](#dolphin-tool)
- [Update Frequency](#update-frequency)
    - [Official Releases](#official-releases)
    - [Development Releases](#development-releases)
- [Official Builds](#official-builds)

## How to connect your Wii remotes

Dolphin offers three different methods to play your games using real Wii remotes.

The most straightforward one is to pair the Wii remote to your computer over Bluetooth and use it in the same way you would any other Bluetooth controller. However, connecting it this way prevents many features from working correctly and isn't recommended.

In order to make full usage of your Wii remote you'll want to use one of the other two options:

## Passthrough a Bluetooth adapter

When using this method, Dolphin will take direct control of a USB Bluetooth adapter and use it in the same way a real Wii would.

This method gives the most accurate results, including audio support on the controller, but has two main drawbacks:

- Requires a custom udev rule
- Hardware compatibility is limited to a few models.

There's no practical way of installing a udev rule from within a Flatpak (at least not without going against flathub rules), so the user must do this manually.

The project's wiki has information on how to set up the udev rule:

https://wiki.dolphin-emu.org/index.php?title=Bluetooth_Passthrough#Linux

Compatibility table:

https://wiki.dolphin-emu.org/index.php?title=Bluetooth_Passthrough#Adapter_test_results

## Emulate the Wii's Bluetooth adapter

This method isn't as accurate as passthrough, but it has much better hardware compatibility and doesn't require installing any udev rules.

It only requires ```bluez``` which is bundled with the Flatpak, and ```allow=bluetooth``` which is enabled by default in the manifest. Coupled with the improved compatibility this means it should work outside the box for most users.

## How to enable motion controls on non-Wii controllers

Some popular controllers such as those on the nintendo switch and ps4/ps5 feature motion sensors that can be used to approximate some Wii remote features.

Even though his bundle already ships with the necessary dependencies, depending on your distribution you may still need to manually add a udev rule to allow applications to access the motion sensors:

```SUBSYSTEM=="input", KERNEL=="event*", ATTRS{name}=="*Motion Sensors", TAG+="uaccess"```

Fedora users should place this rule under ```/etc/udev/rules.d/```, it should also be the same in most other systems but it could also have slight variations from one distribution to another.

## Permissions Used

### device=all

This is required in order for arbitrary gamepads to work, as well as GPU acceleration.

If you don't like this and don't want gamepad support it is possible to change this to ```device=dri``` so that OpenGl will still work.

### filesystem=host:ro

Grants read-only access to the host file system. Dolphin requires this in order to display the contents of your games directory on the main window when running on distributions shipping old libraries (tested on debian 10).

You can safely disable this on reasonably modern distributions.

### socket=pulseaudio

Dolphin needs this to play audio.

### env=QT_QPA_PLATFORM=xcb

This env variable fixes a hypothetical case that could prevent Dolphin from running if it were set to wayland, it's unlikely but we keep it just in case.

Usually this would only happen if the user had globally set the variable in order to force qt applications to run on native wayland mode, otherwise it is safe to drop.

### socket=x11

Necessary in order to display the window, Dolphin has no wayland support at this point.

### share=network

It's required by some features to work, such as netplay, firmware updates and the optional telemetry.

It's safe to disable if you don't want those features.

## share=ipc

Graphical applications will run slowly without this.

### allow=bluetooth

Only necessary when using "Real Wii remotes" in conjunction with the feature labeled "Emulate the Wii's Bluetooth adapter".

It's safe to disable if that feature is not in use, generic bluetooth gamepads will still work without it. Actual Wii remotes can also work without this option when using the separate passthrough feature.

### filesystem=xdg-run/app/com.discordapp.Discord:create

Necessary for discord (a nonfree messaging service) integration.

Can be safely dropped if discord is not used.

### filesystem=xdg-run/gamescope-0:ro

Necessary for HDR10 support through gamescope.

At the moment the only realistic usage case for this are HDR10 filters if you have an oled steam deck, it can safely be dropped in most other cases.

### talk-name=org.freedesktop.ScreenSaver

Required for screensaver inhibition during gameplay.

It can be disabled but your screensaver might trigger during gameplay depending on your input device and screensaver configuration.

## dolphin-tool

Some cli iso manipulation tasks can be achieved with `dolphin-tool`, it is bundled with the Flatpak but not exposed outside of the Flatpak sandbox.

It can be accessed through the `--command` option in `flatpak run`, for instance, checking a game's header would be achieved in this way:

`flatpak run --command=dolphin-tool org.DolphinEmu.dolphin-emu header -i /path/to/file`

## Update Frequency

### Official Releases

The Flatpak updates in accordance to the official "Releases" on the [Dolphin website](https://dolphin-emu.org/download/) and [Dolphin progress reports](https://dolphin-emu.org/blog/) (these usually happen at the same time).

### Development Releases

The Flatpak pushes "Development" releases regularly to the Flathub Beta repository. These versions are in accordance to the "Development" releases on the [Dolphin website](https://dolphin-emu.org/download/). Note that even though these versions may release frequently, the Flatpak will not update on every "Development" release. 

For instructions on how to add the Flathub Beta repository, see [https://docs.flathub.org/docs/for-users/installation/](https://docs.flathub.org/docs/for-users/installation/). If you would like to use netplay, it is highly recommended you use the official "Release" build instead. The "Development" build updates frequently making it difficult for players to match versions. 

If you have both the official "Release" and the "Development" release installed simultaneously, you may set the active version with the following command (Beta referring to the "Development" branch and Stable referring to the official "Releases"):

```
flatpak make-current org.DolphinEmu.dolphin-emu <beta|stable>
```

## Official Builds

As of `2409-260` official dev builds are available from this repo:

https://flatpak.dolphin-emu.org/dev.flatpakrepo

Single file bundles for `2409-260+` versions are available from the downloads section of the official dolphin site:

https://dolphin-emu.org/download/
