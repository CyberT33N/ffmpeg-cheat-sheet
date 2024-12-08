# ffmpeg-cheat-sheet
FFMPEG Cheat Sheet with the most needed stuff..


# transform


## Resize

<br><br>

### Change width to 720
```
ffmpeg -i input.mp4 -vf "scale=720:trunc(ow/a/2)*2" -c:v libx264 -crf 23 -preset medium -c:a aac -b:a 128k output.mp4
```

### Change width to 314:328
```
ffmpeg -i input.mp4 -vf "scale=314:328" -c:a copy output.mp4
```







<br><br>
<br><br>

## Rotate
```shell
# Here, replace <x> with 0 to disable any existing rotation, or any value like 90, 180 or 270 to rotate the displayed video. Note that some players may ignore these flags.
ffmpeg -i input.mp4 -c copy -metadata:s:v:0 rotate=<x> output.mp4
```












<br><br>
<br><br>

# images

<br><br>



# Split video into pictures
```shell
ffmpeg -i file.mpg -r 1/1 $filename%03d.bmp
```










<br><br>
<br><br>

# Convert

## mp4 to webm
```
ffmpeg -i input.mp4 -c:v libvpx-vp9 -b:v 1M -c:a libopus output.webm
```






<br><br>
<br><br>


# Optimize

## Option #1 (High compress) and Resize to 1280x720
```shell
ffmpeg -i input.mp4 -vcodec libx264 -preset slow -profile:v high -level 4.1 -b:v 2500k -maxrate 2500k -bufsize 5000k -vf "scale=-2:720" -acodec aac -b:a 128k -movflags +faststart output.mp4
```

If you get error: `x264 [error]: high profile doesn't support 4:2:2`
```shell
ffmpeg -i 0_Effect_Fluorescent_3840x2160.mov -vcodec libx264 -preset slow -profile:v high422 -level 4.1 -b:v 2500k -maxrate 2500k -bufsize 5000k -vf "scale=-2:720" -acodec aac -b:a 128k -movflags +faststart output.mp4
```

If you get error: `264 [error]: high profile doesn't support 4:4:4`
```shell
ffmpeg -i 5496506_Coll_wavebreak_Animation_1920x1080.mov -vcodec libx264 -preset slow -profile:v high444 -level 4.1 -b:v 2500k -maxrate 2500k -bufsize 5000k -vf "scale=-2:720" -acodec aac -b:a 128k -movflags +faststart output.mp4
```





Erklärung des Befehls:
1. **`-i input.mp4`**  
   Das Eingabevideo. Ersetze `input.mp4` durch den Pfad deiner Videodatei.

2. **Video-Codec: `-vcodec libx264`**  
   Nutzt den H.264-Codec für maximale Kompatibilität.

3. **Preset: `-preset slow`**  
   Steuert die Geschwindigkeit der Komprimierung:  
   - *slower*: bessere Kompression, längere Verarbeitungszeit.  
   - *faster*: weniger Kompression, schneller.

4. **Profil & Level: `-profile:v high -level 4.1`**  
   Stellt sicher, dass das Video auf den meisten Browsern und Geräten abspielbar ist.

5. **Bitrate: `-b:v 2500k -maxrate 2500k -bufsize 5000k`**  
   Setzt eine feste Zielbitrate (hier: 2.5 Mbps). Anpassbar an deine gewünschte Qualität.

6. **Scaling: `-vf "scale=-2:720"`**  
   Skaliert das Video auf eine Höhe von 720 Pixel, während die Breite proportional bleibt.

7. **Audio: `-acodec aac -b:a 128k`**  
   Kodiert den Ton in AAC (128 kbps).

8. **Faststart: `-movflags +faststart`**  
   Verschiebt die Metadaten des MP4-Videos an den Anfang der Datei, damit das Video beim Laden direkt abgespielt werden kann.

9. **Output: `output.mp4`**  
   Name der optimierten Datei.

### Anpassungen:
- **Für höhere Auflösung (1080p):**
  Ändere `-vf "scale=-2:720"` zu `-vf "scale=-2:1080"` und erhöhe die Bitrate (`-b:v 5000k`).
- **Für AV1-Codec (effizienter, aber langsamer):**
  Ersetze `libx264` durch `libaom-av1`.

Dieser Befehl liefert dir ein weboptimiertes Video mit ausgezeichneter Balance aus Qualität und Ladezeit.








<br><br>
<br><br>


## Option #2 (Max compress) and Resize to 1280x720
Für die kleinste Dateigröße bei akzeptabler Qualität kannst du die **CRF-Methode (Constant Rate Factor)** verwenden. Sie passt die Bitrate automatisch an, um die Qualität zu erhalten, ohne die Datei unnötig groß zu machen.  

Hier der optimierte FFmpeg-Befehl:  

```bash
ffmpeg -i input.mp4 -vcodec libx264 -preset veryslow -crf 28 -profile:v high -level 4.1 -vf "scale=-2:720" -acodec aac -b:a 96k -movflags +faststart output.mp4
```

### Änderungen gegenüber dem vorherigen Befehl:
1. **CRF (Qualitätsfaktor): `-crf 28`**  
   - Der CRF-Wert steuert die Kompression.  
   - **Empfohlene Werte:**  
     - 18–23: Hohe Qualität, größere Dateien.  
     - 24–28: Gute Qualität, kleinere Dateien.  
     - Ab 30: Sichtbarer Qualitätsverlust.  

2. **Preset: `-preset veryslow`**  
   - Verwendet mehr Zeit für die Kodierung, um die beste Kompression zu erreichen.  
   - Kann auf `slow` reduziert werden, wenn die Kodierung zu lange dauert.

3. **Audio-Bitrate: `-b:a 96k`**  
   - Niedrigere Bitrate für Audio, spart Platz (reicht für Sprache und einfache Musik).

4. **Skalierung: `scale=-2:720`**  
   - Auflösung bleibt bei 720p für Weboptimierung.

### Beispiel:
Ein 100 MB großes 1080p-Video könnte mit diesem Befehl auf ~10–20 MB reduziert werden, je nach Inhalt und Bewegung.  

**Falls noch kleinere Dateien gewünscht sind:**
- Erhöhe den CRF-Wert auf 30.
- Reduziere die Audio-Bitrate auf `-b:a 64k`.  

Teste die Qualität nach der Komprimierung, um sicherzugehen, dass sie für deinen Zweck noch ausreichend ist.
