# closed-captions-reader
uses Tesseract.js and ffmpeg to recognize subtitle text

# Setup
- npm install

This uses a dependency
- ffmpeg
- espeak

## How it works
We take mjpeg-server and run a ffmpeg stream over it. Its function is to serve as screen recording. 

The command for that is:
`
 ./mjpeg-server -d ffmpeg \-loglevel debug \-probesize 32 \-fpsprobesize 0 \-analyzeduration 0 \-fflags nobuffer \-f x11grab \-r 1 \-video_size 1366x768 \-i :0 \-f mpjpeg \-q 2 \-^
`
Next, in the client, we run a mjpeg decoder to save a image from https://localhost:9000, and then use fs to save the image.
The image is now in ./mjpeg/static.jpg.

with the decoding done, we can move on to the character reconition. using the Tesseract library we can load the image and it shows out the text.

using a node child process, we perform a system call to espeak passing in the ocr text from Tesseract as the argument.

## TODO
- currently running at 1fps and 400s of latency.
- optimize the streaming pipline
- tts should stop reading after new  to get rid of audio layering.
