# MacroPad

This is a tool for turning any key-based input device into a "macropad" for
triggering commands on your PC. Although designed with traditional keyboard
numpads in mind, MacroPad will work with any device that fires keycodes.

MacroPad intercepts keycodes on a per-device basis, meaning multiple keyboards
can be plugged in and configured independently.

MacroPad's most powerful feature is layering, which allows keys to change the
mapping of the device on the fly.

### Changelog

* 01-04-2020 - Alternate detection method now supports bluetooth devices. This is
  checked during `--detect` and opens devices based on their name instead of
  `by-id` path. As a result of supporting bluetooth, reconnect timeouts/waits
  are in place.

## WIP / Requesting Help

Documentation lags behind the current featureset. I update this readme and the
linked tutorials as often as possible, but it's a low priority.

For those who like learning by example, I've included my
[personal config](example.conf). It almost always has examples of new
functionality.

If you're having trouble setting up a config, open up an issue and I will help.

## Requirements

* An extra USB keyboard or numpad (see `Notes / Quirks`)
* python3
* python-evdev
* `xdotool` - used for `TYPE` command
* `input` group membership

## Notes / Quirks

* By default, MacroPad consumes ALL events generated by a device. This is done
  to place emphasis on using a device only for macroing, like a standalone
  numpad. Override this by placing `PASSTHROUGH` in the `DEVICE` section.

## Setup

First, ensure you have the `python-evdev` module installed via `pip` or your
distro's package manager. In Arch Linux, use `python-evdev`. If you aren't
already, add yourself to the `input` group.

Clone this repo to a directory of your choosing, then run:

`./macropad.py --detect`

Follow the on-screen instructions. Note the location of the configuration file,
then open it in the text editor of your choosing.

### Learning / Documentation

Please see [advanced usage](config-documentation.md) after reading the section
below.

### Configuration Workflow (Basic)

Open up a terminal alongside your text editor and run:

`./macropad.py --show /path/to/config/file`

Now press any key on the input device chosen in the last step. You will see
something along the lines of:

```
KEY_KP1 - KEY_DOWN
KEY_KP1 - KEY_HOLD
KEY_KP1 - KEY_HOLD
KEY_KP1 - KEY_HOLD
KEY_KP1 - KEY_UP
```

(output will differ based on key and duration of the key press)

Take note that numpads will often fire KEY_NUMPAD in addition to the key being
pressed. This can be ignored.

To configure a key, follow this format:

```

BINDS {
	<KEYCODE> {
		.. options ..

		<EVENT> {
			.. commands ..
		}
	}
}
```

Formatting and indents matter!

Supported events are:

```
ON_PRESS
ON_RELEASE
```

Where `commands` is:

```
RUN <program/script/etc>
TYPE <string>
KEY <keycode>
LAYER <layer>
HOTLAYER <layer>
MODELAYER <layer>
```

`options` only supports one command at the time of writing:

`BIND <keycode>`

(this is a shortcut to doing `ONPRESS [...] KEY <keycode>`)

Save the config file. Now run:

`./macropad /path/to/config/file`

# Example Config

I've attached my personal config file to this repo: [example.conf](example.conf).
