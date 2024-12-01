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
ffmpeg -i input.mp4 -vcodec libx264 -preset slow -profile:v high -level 4.1 -b:v 2500k -maxrate 2500k -bufsize 5000k -vf "scale=-2:720" -acodec aac -b:a 128k -movflags +faststart output.mp4
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
```
