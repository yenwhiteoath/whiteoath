---
title: Unix scripts
subtitle: A collection of useful Unix shell scripts
date: 2022-10-07
tags: ['computing']
toc: true
draft: false
---

## convert

rotate

`convert image.png -rotate "90" output.png`

crop

`convert image.png -crop 640x480+50+100 +repage output.jpg`

resize to percent

`convert image.png -geometry 50% output.jpg`

resize width to pixels

`convert image.png -geometry 1024x output.jpg`

resize height to pixels

`convert image.png -geometry x800 output.jpg`

tile all images in folder

`montage * output.jpg`

convert pdf to jpg

`convert file.pdf -flatten output.jpg`

create gif, 2 sec delay between frames

`convert -delay 200 -loop 0 *.jpg output.gif`

dither of an image

`convert image.png -colorspace gray -ordered-dither o8x8 output.png`

optimized dither

`convert input.png -filter Triangle -define filter:support=2 -thumbnail 1200 -unsharp 0.25x0.08+8.3+0.045 -dither FloydSteinberg -type Grayscale -colors 2 -posterize 136 -quality 82 -define jpeg:fancy-upsampling=off -define png:compression-filter=5 -define png:compression-level=9 -define png:compression-strategy=1 -define png:exclude-chunk=all -interlace none -colorspace sRGB output.png`

## ffmpeg/youtube-dl

convert audio files

`for file in *.m4a; do ffmpeg -i "${file}" "${file/%m4a/ogg}" && rm $file; done`

mp4 to webm

`find "./" -name '*.mp4' -exec sh -c 'ffmpeg -i "$0" -c:v libvpx -crf 10 -b:v 2M -c:a libvorbis "${0%%.mp4}.webm"' {} \;`

mkv to timestamped webm with subtitles

`ffmpeg -i test.mkv -ss 00:22:08.00 -to 00:22:49.00 -f webm -c:v libvpx -b:v 1M -map 0:v -map 0:a:1 -vf subtitles=test.mkv -acodec libvorbis example.webm -hide_banner`

download best mp3

`yt-dlp --add-metadata -i -x -f mp3/bestaudio "video_link"`

download playlist

`yt-dlp -o '%(playlist_index)s - %(title)s.%(ext)s' -x -f mp3/bestaudio "[playlist url]"`

## grep

find all files containing text pattern (recursive/line number/match whole word)

`grep -rnw '/path/to/somewhere/' -e 'pattern'`

replace text recursively under directory

`grep -rl oldtext . | xargs sed -i 's/oldtext/newtext/g'`

## wget

archive - recursive download of website including files

`wget -mkEpnp http://example.org`