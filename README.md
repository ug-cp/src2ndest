# `rsync2multi`

> usage: rsync2multi [OPTION...] SRC [DEST...]

This is a small and simple bash script to do a `rsync` from one `SRC`
to multiple `DEST`.

The principle is simple:

Every argument starting with "-" is interpreted as an option for `rsync`.
Every argument not starting with "-" is interpreted as a path.
The first path is used as `SRC` and the other paths are used as
destinations `DEST`.

This means you can not give a parameter to a flag directly.
For example, instead of '-B 512' use '--block-size=512' or '"-B 512"'.
Or instead of '-e "ssh -p 1234"' use '--rsh="ssh -p 1234"'.

## Installation

To use it, simply copy it to your path, e. g.:

```sh
install -p -o root -g root -m 755 rsync2multi /usr/local/bin/rsync2multi
```

Or if you only want to install it in your user space:

```sh
install -p -m 755 rsync2multi ~/bin/rsync2multi
```

## Examples

Example:

```sh
rsync2multi -a -r --delete --exclude=.git -v /foo/ /bar/ /baz/
```

This syncs the directory `/foo/` to the two destinations `/bar/` and `/baz/`.
This is similar to:

```sh
rsync -a -r --delete --exclude=.git -v /foo/ /bar/
rsync -a -r --delete --exclude=.git -v /foo/ /baz/
```

Instead of `rsync2multi` you can also use the much more general tool `xargs`:

```sh
echo /bar/ /baz/ | xargs -n 1 rsync -a -r --delete --exclude=.git -v /foo/
```

Instead of `rsync2multi` you can also use the much more general tool `parallel`
which runs `rsync` in parallel:

```sh
parallel rsync -a -r --delete --exclude=.git -v /foo/ ::: /bar/ /baz/
```

Example:

```sh
rsync2multi -a -r "-e 512" /foo/ /bar/
```

This syncs the directory `/foo/` to the directory `/bar/`.
This is equivalent to:

```sh
rsync -a -r "-e 512" /foo/ /bar/
```

## License

    Copyright (C) 2022  Daniel Mohr

    This program is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

    You should have received a copy of the GNU General Public License
    along with this program.  If not, see <https://www.gnu.org/licenses/>.
