---
author: Daniel Mohr
date: 2022-12-06
license: GPLv3 or any later version
home: https://gitlab.com/ug-cp/fast_samd21_tc
mirror: https://github.com/ug-cp/fast_samd21_tc
---

# `src2ndest`

> usage: src2ndest PRG [OPTION...] SRC [DEST...]

`src2ndest` is a small and simple bash script to call `PRG` for one `SRC` and
multiple `DEST`.
For example using `cp` or `rsync` as `PRG` you can copy files from on source
directory to multiple destination directories.

The principle is simple:

Every argument starting with "-" is interpreted as an option.
Every argument not starting with "-" is interpreted as a `PRG`, `SRC` or `DEST`.
The first one is used as `PRG`. The second one as `SRC`. And the remaining ones
as destinations `DEST`.

This means you can not give a parameter to a flag directly.
For example, instead of '-B 512' use '--block-size=512' or '"-B 512"'.
Or instead of '-e "ssh -p 1234"' use '--rsh="ssh -p 1234"'.

If an error arises `src2ndest` terminates with the exit status of the of the
failed command. For example if you use `rsync` to sync to 3 destinations and
the second destination does not exists at all, the first destination gets
synced and the `src2ndest` terminates with the exit status of `rsync` for the
second destination.

## Installation

This script only needs the bash. If you do not have the bash installed,
install it.

To use this bash script, simply copy it to your path, e. g.:

```sh
install -p -o root -g root -m 755 src2ndest /usr/local/bin/src2ndest
```

Or if you only want to install it in your user space:

```sh
install -p -m 755 src2ndest ~/bin/src2ndest
```

If you want `src2ndesttol`, which is more tolerant and not terminates if an
error arises, you can create and install it:

```sh
add-on/generate_tolerate_patch
install -p -m 755 src2ndesttol ~/bin/src2ndesttol
```

## Examples

Example 1:

```sh
src2ndest cp foo bar/ baz/
```

This copies the file foo to the directories bar/ and baz/ and is the same as:

```sh
cp foo bar/
cp foo baz/
```

Example 2:

```sh
src2ndest rsync -a -r --delete --exclude=.git -v /foo/ /bar/ /baz/
```

This syncs the directory `/foo/` to the two destinations `/bar/` and `/baz/`.
The destinations are handled by `rsync` and therefore they could be
everything `rsync` handles (e. g. remote shell like ssh or `rsync` daemon).
This is similar to:

```sh
rsync -a -r --delete --exclude=.git -v /foo/ /bar/
rsync -a -r --delete --exclude=.git -v /foo/ /baz/
```

Instead of `src2ndest` you can also use the much more general tool `xargs`:

```sh
echo /bar/ /baz/ | xargs -n 1 rsync -a -r --delete --exclude=.git -v /foo/
```

Unfortunately handling filenames containing blanks is not easy with `xargs`.

Instead of `src2ndest` you can also use the much more general tool `parallel`
which runs `rsync` in parallel:

```sh
parallel rsync -a -r --delete --exclude=.git -v /foo/ ::: /bar/ /baz/
```

Example 3:

```sh
src2ndest rsync -a -r "-e 512" /foo/ /bar/
```

This syncs the directory `/foo/` to the directory `/bar/` with some parameters.
This is equivalent to:

```sh
rsync -a -r "-e 512" /foo/ /bar/
```

## License

```txt
    src2ndest
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
```
