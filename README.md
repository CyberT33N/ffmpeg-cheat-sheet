# ffmpeg-cheat-sheet
FFMPEG Cheat Sheet with the most needed stuff..



<br><br>

# Rotate
```shell
# Here, replace <x> with 0 to disable any existing rotation, or any value like 90, 180 or 270 to rotate the displayed video. Note that some players may ignore these flags.
ffmpeg -i input.mp4 -c copy -metadata:s:v:0 rotate=<x> output.mp4
```


<br><br>
<br><br>


# Split video into pictures
```shell
ffmpeg -i file.mpg -r 1/1 $filename%03d.bmp
```



<br><br>
<br><br>


# Optimize for web
```shell
ffmpeg -i 0_Woman_Red_3840x2160.mov -vcodec libx264 -preset slow -profile:v high -level 4.1 -b:v 2500k -maxrate 2500k -bufsize 5000k -vf "scale=-2:720" -acodec aac -b:a 128k -movflags +faststart output.mp4
```
