# FFmpeg_notes
FFmpeg command list

".vid" = Any video format supported
".aud" = Any audio format supported

## Insert audio on video
**$ ffmpeg -i a_input.aud -i v_input.vid -codec copy -shortest v_output.vid**

_Comments: Seems not to work if you put the audio file as the second input parameter_

## Remove audio from video
**$ ffmpeg -i input.vid -c copy -an output_noaudio.vid**

## Concatenate video files
**$ ffmpeg -f concat -safe 0 -i v_input1.vid -i v_input2.vid -c copy v_output.vid**

## Crop video (in frame dimension)
**$ ffmpeg -i input.mp4 -filter:v "crop=80:60:200:100" -c:a copy output.mp4**

_80:60 - Crop window size in pixes_
_200:100 - Starting from x=200, y=100 (on original image)_

## Convert ogv to mp4
**$ ffmpeg -i input.ogv -c:v libx264 -preset veryslow -crf 22 -c:a libmp3lame -qscale:a 2 -ac 2 -ar 44100 output.mp4**

## Convert video to gif
This example will skip the first 30 seconds of the input and create a 3 second output. It will scale the output to be 320 pixels wide and automatically determine the height while preserving the aspect ratio. The palettegen and paletteuse filters will generate and use a custom palette generated from your source. (source: https://superuser.com/questions/556029/how-do-i-convert-a-video-to-gif-using-ffmpeg-with-reasonable-quality)

### Generate a palette:
**$ ffmpeg -y -ss 30 -t 3 -i input.flv -vf fps=10,scale=320:-1:flags=lanczos,palettegen palette.png**
### Output the GIF using the palette:
**$ ffmpeg -ss 30 -t 3 -i input.flv -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" output.gif**

### Other option
**$ ffmpeg -i video.avi video.gif -hide_banner**
