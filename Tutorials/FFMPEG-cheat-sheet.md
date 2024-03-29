# FFMPEG Cheat Sheet
This file details a set of common things you may want to do with FFMPEG.

## Create a video from a sequence of PNG images
The following command can be used to make a .mp4 video from a series of images padded with zeros (`pic.00001.png`, `pic.0002.png`, ...).

```
ffmpeg -r 30 -f image2 -i pic.%04d.png -vcodec libx264 -crf 25  -pix_fmt yuv420p test.mp4
```

The inputs are:
* `-r` is the framerate
* `-crf` is the quality, lower means better quality
* `-S` (optional) video resolution in pixes
* `-pix_fmt` specifies the pixel format

## Playing one video after another
The following command can be used to play a sequence of videos with the same width and height.
```
ffmpeg -f concat -i videos.txt -c copy joinedVideo.mp4
```
The file `videos.txt` contains an ordered list of videos with the following syntax: 
```
file '/directory/Video1.mp4'
file '/directory/Video2.mp4'
```
## Stacking videos
The following command can be used to vertically stack two videos with the same width.
```
ffmpeg -i 1.mp4 -i 2.mp4 -filter_complex vstack out.mp4
```
The following command can be used to horizontally stack two videos with the same height.
```
ffmpeg -i 1.mp4 -i 2.mp4 -filter_complex hstack out.mp4
```
`N` videos can be stacked using the following.
```
ffmpeg -i 1.mp4 -i 2.mp4 -i 3.mp4 -filter_complex hstack=N out.mp4
```
A 2x2 array of compatible videos can be created using
```
ffmpeg -i input0 -i input1 -i input2 -i input3 -filter_complex "[0:v][1:v][2:v][3:v]xstack=inputs=4:layout=0_0|w0_0|0_h0|w0_h0[v]" -map "[v]" output.mp4
```

## Dealing with an odd number of pixels
Adding `-vf "pad=ceil(iw/2)*2:ceil(ih/2)*2"` to your FFMPEG command as shown below allows you to create a video from PNGs that are not an even number of pixels in height and/or width.
```
ffmpeg -r 30 -f image2 -i pic.%04d.png -vcodec libx264 -vf "pad=ceil(iw/2)*2:ceil(ih/2)*2" -crf 25  -pix_fmt yuv420p test.mp4
```

## Adding an .mp3 to a video
The following command can be used to add sound to video.
```
ffmpeg -r 30 -f image2 -i pic.%04d.png -i MP3FILE.mp3 -vcodec libx264 -b 4M -vpre normal -acodec copy test.mp4
```
The inputs are:
* `-i MP3FILE.mp3` is the name of the audio file
* `-acodec copy` copies the audio from the input to the output stream

## Converting a video to .mp4
The following command can be used to convert a video to .mp4.
```
ffmpeg  -i INPUT.avi -vcodec libx264 -crf 25 OUTPUT.mp4
```

## Cropping a video
The following command can be used to crop a video
```
ffmpeg -i input.mp4 -vf "crop=w:h:x:y" output.mp4
```
where the inputs are
* `-vf` indicates the usage of a video filter
* `crop` is the name of the filter
* `w:h` is the width and height of the output video<
* `x:y` is the coordinate from which the video will be