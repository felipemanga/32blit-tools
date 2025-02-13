# 32blit Tools

[![Build Status](https://shields.io/github/workflow/status/32blit/32blit-tools/Python%20Tests.svg)](https://github.com/32blit/32blit-tools/actions/workflows/test.yml)
[![Coverage Status](https://coveralls.io/repos/github/32blit/32blit-tools/badge.svg?branch=master)](https://coveralls.io/github/32blit/32blit-tools?branch=master)
[![PyPi Package](https://img.shields.io/pypi/v/32blit.svg)](https://pypi.python.org/pypi/32blit)
[![Python Versions](https://img.shields.io/pypi/pyversions/32blit.svg)](https://pypi.python.org/pypi/32blit)

This toolset is intended for use with the 32Blit console to prepare assets and upload games.

# Running

The 32Blit toolset contains subcommands for each tool, you can list them with:

```
32blit --help
```

* image - Convert images/sprites for 32Blit
* font - Convert fonts for 32Blit
* map - Convert popular tilemap formats for 32Blit
* raw - Convert raw/binary or csv data for 32Blit
* pack - Pack a collection of assets for 32Blit
* cmake - Generate CMake configuration for the asset packer
* flash - Flash a binary or save games/files to 32Blit
* metadata - Tag a 32Blit .blit file with metadata
* relocs - Prepend relocations to a game binary
* version - Print the current 32blit version

To run a tool, append its name after the `32blit` command, eg:

```
32blit version
```

## Tools

### Metadata

Build metadata, and add it to a `.blit` file.

### Flash

Flash and manage games on your 32Blit over USB serial.

### Relocs

Collate a list of addresses that need patched to make a `.blit` file relocatable and position-independent.

### Cmake

Generate CMake files for metadata information and/or asset pipeline inputs/outputs.

## Assets

You will typically create assets using the "asset pipeline", configured using an `assets.yml` file which lists all the files you want to include, and how they should be named in code.

An `assets.yml` file might look like:

```yml
# Define an output target for the asset builder
# in this case we want a CSource (and implicitly also a header file)
# type auto-detection will notice the ".cpp" and act accordingly
assets.cpp:
  prefix: asset_
  # Include assets/sprites.png
  # and place it in a variable named "asset_sprites"
  # Since it ends in ".png" the builder will run "sprites_packed" to convert our source file
  assets/sprites.png:
    name: sprites
    palette: assets/sprites.act
    strict: true  # Fail if a colour does not exist in the palette
    transparent: 255,0,255

  # Include assets/level.tmx
  # and place it in a variable named "asset_level_N_tmx"
  # Since it ends in ".tmx" the builder will run "map_tiled" to convert our source file
  assets/level*.tmx:
```

### Fonts

Converts a ttf file or image file into a 32Blit font.

Supported formats:

* Image .png, .gif
* Font .ttf

### Images

All image assets are handled by Pillow so most image formats will work, be careful with lossy formats since they may add unwanted colours to your palette and leave you with oversized assets.

Supported formats:

* 8bit PNG .png
* 24bit PNG .png

Options:

* `palette` - Image or palette file (Adobe .act, Pro Motion NG .pal, GIMP .gpl) containing the asset colour palette
* `transparent` - Transparent colour (if palette isn't an RGBA image), should be either hex (FFFFFF) or R,G,B (255,255,255)
* `packed` - (Defaults to true) will pack the output asset into bits depending on the palette size. A 16-colour palette would use 4-bits-per-pixel.
* `strict` - Only allow colours that are present in the palette image/file

### Maps/Levels

Supported formats:

* Tiled .tmx - https://www.mapeditor.org/ (extremely alpha!)

### Raw Binaries/Text Formats

Supported formats:

* CSV .csv
* Binary .bin, .raw
