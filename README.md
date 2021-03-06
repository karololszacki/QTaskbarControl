# QTaskbarControl
A class to create a taskbar/launcher progress and more, for all desktop platforms

## Features
- Show a progress in the taskbar/launcher/dock/…
- Show a badge with a number in the taskbar/launcher/dock/…
- Works on:
	- Windows >= 7
	- macOS
	- Linux (Desktops that support the `com.canonical.Unity.LauncherEntry` D-Bus signal, for example Unity, KDE)
- Additional Windows Features:
	- Set the badge icon background and text color
	- Set the progress state (running, paused, stopped)
	- Show indeterminate progress

## Installation
The package is providet as qpm package, [`de.skycoder42.qtaskbarcontrol`](https://www.qpm.io/packages/de.skycoder42.qtaskbarcontrol/index.html). To install:

1. Install qpm (See [GitHub - Installing](https://github.com/Cutehacks/qpm/blob/master/README.md#installing), for **windows** see below)
2. In your projects root directory, run `qpm install de.skycoder42.qtaskbarcontrol`
3. Include qpm to your project by adding `include(vendor/vendor.pri)` to your `.pro` file

Check their [GitHub - Usage for App Developers](https://github.com/Cutehacks/qpm/blob/master/README.md#usage-for-app-developers) to learn more about qpm.

**Important for Windows users:** QPM Version *0.10.0* (the one you can download on the website) is currently broken on windows! It's already fixed in master, but not released yet. Until a newer versions gets released, you can download the latest dev build from here:
- https://storage.googleapis.com/www.qpm.io/download/latest/windows_amd64/qpm.exe
- https://storage.googleapis.com/www.qpm.io/download/latest/windows_386/qpm.exe

## Usage
For most platforms, you simply need to create an instance of the class for a window, and your good to go. For linux however, you need to do additional setup. Read [Special setup for linux](#special-setup-for-linux). The following example shows the part thats the same for all platforms:

```cpp
auto widget = new QWidget(); //your window
auto taskbar = new QTaskbarControl(widget); //MUST have a valid parent!

// additional linux setup here

taskbar->setProgress(0.42);
taskbar->setProgressVisible(true);
taskbar->setCounter(42);
taskbar->setCounterVisible(true);
```

### Special setup for linux
In order to work, the linux implementation needs an application with a valid desktop file in an accessible location (e.g. `~/.local/share/applications/` on KDE). You need to set that file as extra property to enable it. The following steps explain how to properly setup QTaskbarProgress with a desktop file:

1. Create a `.desktop` file. For testing purpose, the following should suffice:
```.desktop
[Desktop Entry]
Type=Application
Version=1.1
Name=<MyApp>
Exec=<path_to_MyApp>
```
2. Copy that file to a location where desktop files are found by the OS (e.g. `~/.local/share/applications`, `/usr/local/share/applications`, `/usr/share/applications`)
3. Register the desktop file in your application. In case the file is named `myapp.desktop`, add the following code after the construction of QTaskbarControl:
```cpp
taskbar->setAttribute(QTaskbarControl::LinuxDesktopFile, "myapp.desktop");
```

And thats it. When you run the application it should work. I case it doesn't work: Try to set a different icon for the desktop file, e.g. `gtk-edit`. If you see that icon in the taskbar, your application gets corretly "connected" to that desktop file.

## Icon Reference
- Windows: https://www.iconfinder.com/icons/10282/circle_red_icon#size=48
