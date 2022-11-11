# ffmpeg-cheat-sheet
FFMPEG Cheat Sheet with the most needed stuff..



<br><br>

# Rotate
```shell
# Here, replace <x> with 0 to disable any existing rotation, or any value like 90, 180 or 270 to rotate the displayed video. Note that some players may ignore these flags.
ffmpeg -i input.mp4 -c copy -metadata:s:v:0 rotate=<x> output.mp4
```
