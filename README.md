#### The only notable difference of this fork is that it doesn’t depend on pyEnchant anymore, which has been discontinued and doesn’t support 64 bit.

A python program to OCR videos

Adapted (aka shamelessly stolen) from the bash based video OCR Yolocr (https://git.clapity.eu/Id/YoloCR)

It uses vapoursynth, ffmpeg and tesseract along with some python modules


Requirements
============

Install 
- [python 3+](https://www.python.org/downloads/release) (Use the 32-bit version on Windows, because pyEnchant does not support Win64)
- [vapoursynth](https://github.com/vapoursynth/vapoursynth/releases)
- [tesseract](https://github.com/tesseract-ocr/tesseract/wiki/Downloads).
(Make sure to use the [official training data](https://github.com/tesseract-ocr/tesseract/wiki/Data-Files). The one included in the Windows setup by UB Mannheim is faulty and needs to be replaced.)
- (optionnal) [Vapoursynth Editor](https://bitbucket.org/mystery_keeper/vapoursynth-editor/downloads/)

Python, ffmpeg and vspipe (vapoursynth) should be in the PATH

Install vapoursynth plugins:
- For Windows:
  1. Unzip the content of dependecies/Win/vapoursynth_dep.zip to your vapoursynth plugin folder
  2. Unzip the content of dependecies/Win/python_dep.zip to your python site-packages folder   
- For Linux:
  - install [Vapoursynth-plugins](https://github.com/darealshinji/vapoursynth-plugins) (or use my containerized version of pythoCR)

Install (from pip) python prerequisites:

`$pip3 install colorama configargparse spellchecker numpy opencv-python tqdm`

Installation
============

clone this repository

Here, you're done.

How to use
==========


```
$python3 pythOcr.py --help
usage: PythoCR [-h] [--version] [-c CONFIG] [-l language] [-wd folder]
               [-o folder] [--log-level level] [--ass-style style]
               [-rr path to regex-replace json] [-hcr char,replace]
               [--sub-format format] [--mode mode] [--vpy vpy_file]
               [--threads number] [--auto-same-sub-threshold number]
               [--same-sub-threshold number] [--no-spellcheck] [-t] [-d]
               [--tesseract-path path to tesseract binary]
               [--vapoursynth-path path to vspipe binary]
               path [path ...]

Filters a video and extracts subtitles as srt or ass. Args that start with
'--' (eg. --version) can also be set in a config file (specified via -c).
Config file syntax allows: key=value, flag=true, stuff=[a,b,c] (for details,
see syntax at https://goo.gl/R74nmi). If an arg is specified in more than one
place, then commandline values override config file values which override
defaults.

positional arguments:
  path                  path to a video

optional arguments:
  -h, --help            show this help message and exit
  --version             show program's version number and exit
  -c CONFIG, --config CONFIG
                        path to configuration file
  -l language, --lang language
                        Select the language of the subtitles (default: fra)
  -wd folder, --work-dir folder
                        Directory where I will put all my temporary stuff
                        (default ./temp)
  -o folder, --output-dir folder
                        Directory where I will put all my released stuff
                        (default ./output)
  --log-level level     Set the logging level (default INFO)
  --ass-style style     ASS style to use if sub-format is ass (default:
                        Verdana 60)
  -rr path to regex-replace json, --regex-replace path to regex-replace json
                        List of regex/replace for automatic correction
  -hcr char,replace, --heuristic-char-replace char,replace
                        List of char/replace for heuristic correction
  --sub-format format   Set the outputed subtitles format (default: srt)
  --mode mode           Set the processing mode. "filter" to only start the
                        filtering jobs, "ocr" to process already filtered
                        videos, "full" for both. (default: full)
  --vpy vpy_file        vapoursynth file to use for filtering (required for
                        "filter only" and "full" modes
  --threads number      Number of threads the script will use (default:
                        automatic detection)
  --auto-same-sub-threshold number
                        Percentage of comparison to assert that two lines of
                        subtitles are automatically the same (default: 95%)
  --same-sub-threshold number
                        Percentage of comparison to assert that two lines of
                        subtitles are the same (default: 80%)
  --no-spellcheck       Deactivate the function which tries to replace
                        allegedly bad characters using spellcheck (it will
                        make the "heurist_char_replace" option of the
                        userconfig useless)
  -t, --timid           Activate timid mode (it will ask for user input when
                        some corrections are not automatically approved)
  -d, --delay           Delay correction after every video is processed
  --tesseract-path path to tesseract binary
                        The path to call tesseract (default: tesseract)
  --vapoursynth-path path to vspipe binary
                        The path to call vapoursynth (default: vspipe)
```

So to process the video /myVideos/vid01.mp4, the command would be `python3 pythoCR.py -c <myconfig> --vpy <myvpy> /myVideos/vid01.mp4`

The vpy file in exemples/ needs to be modified by hand to set the crop parameters to fit your source. 
