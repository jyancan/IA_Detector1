# Handling Input Streams

It's time to really get in the think of things for running your app at the edge. Being able to
appropriately handle an input stream is a big part of having a working AI or computer vision
application. 

In your case, you will be implementing a function that can handle camera, video or webcam
data as input. While unfortunately the classroom workspace won't allow for webcam usage,
you can also try that portion of your code out on your local machine if you have a webcam
available.

As such, the tests here will focus on using a camera image or a video file. You will not need to
perform any inference on the input frames, but you will need to do a few other image
processing techniques to show you have some of the basics of OpenCV down.

Your tasks are to:

1. Implement a function that can handle camera image, video file or webcam inputs
2. Use `cv2.VideoCapture()` and open the capture stream
3. Re-size the frame to 100x100
4. Add Canny Edge Detection to the frame with min & max values of 100 and 200, respectively
5. Save down the image or video output
6. Close the stream and any windows at the end of the application

You won't be able to test a webcam input in the workspace unfortunately, but you can use
the included video and test image to test your implementations.
# Handling Input Streams - Solution

Let's walk through each of the tasks.

> Implement a function that can handle camera image, video file or webcam inputs

The main thing here is just to check the `input` argument passed to the command line.

This will differ by application, but in this implementation, the argument parser makes note
that "CAM" is an acceptable input meaning to use the webcam. In that case, the `input_stream`
should be set to `0`, as `cv2.VideoCapture()` can use the system camera when set to zero.

The next is checking whether the input name is a filepath containing an image file type, 
such as `.jpg` or `.png`. If so, you'll just set the `input_stream` to that path. You should also
set the flag here to note it is a single image, so you can save down the image as part of one
of the later steps.

The last one is for a video file. It's mostly the same as the image, as the `input_stream` is the
filepath passed to the `input` argument, but you don't need to use a flag here.

A last thing you should consider in your app here is exception handling - does your app just
crash if the input is invalid or missing, or does it still log useful information to the user?

> Use `cv2.VideoCapture()` and open the capture stream

```
capture = cv2.VideoCapture(input_stream)
capture.open(args.input)

while capture.isOpened():
    flag, frame = cap.read()
    if not flag:
        break
```

It's a bit outside of the instructions, but it's also important to check whether a key gets 
pressed within the while loop, to make it easier to exit. 

You can use:
```
key_pressed = cv2.waitKey(60)
```
to check for a key press, and then
```
if key_pressed == 27:
    break
```
to break the loop, if needed. Key 27 is the Escape button.

> Re-size the frame to 100x100

```
image = cv2.resize(frame, (100, 100))
```

> Add Canny Edge Detection to the frame with min & max values of 100 and 200, respectively

Canny Edge detection is useful for detecting edges in an image, and has been a useful
computer vision technique for extracting features. This was a step just so you could get a little
more practice with OpenCV.

```
edges = cv2.Canny(image,100,200)
```

> Display the resulting frame if it's video, or save it if it is an image

For video:
```
cv2.imshow('display', edges)
```
For a single image:
```
cv2.imwrite('output.jpg', edges)
```

> Close the stream and any windows at the end of the application

Make sure to close your windows here so you don't get stuck with them on-screen.

```
capture.release()
cv2.destroyAllWindows()
```

I can then test both an image and a video with the following:

```bash
python app.py -i blue-car.jpg
```

```bash
python app.py -i test_video.mp4
```
