Kurento Test Files
==================

These files are used in some of the Kurento unit tests (e.g. to test the PlayerEndpoint), and also in the Kurento Integration tests.



Using files as fake webcams
===========================

Chrome allows using a file as if it were an actual input device. This eases using a specific source to test without needing an actual physical device for the Media Capture API.

There is, however, a strict limitation on what formats are accepted by Chrome. Audio should be uncompressed (i.e. WAV), while video can be either uncompressed as [Y4M](https://wiki.multimedia.cx/index.php?title=YUV4MPEG2), or compressed as MJPEG.

Sample command to scale and convert a video to Y4M:

    ffmpeg \
        -i file.mov \
        -filter:v scale=width=640:height=480,format=yuv420p \
        test.y4m

Sample command to scale and convert a video to MJPEG:

    ffmpeg \
        -i file.mov \
        -filter:v scale=width=640:height=480 \
        test.mjpeg

Sample command to run Chrome or Chromium with a clean profile and fake input device:

    # Uncomment one or the other, depending on which one you want to use.
    #TEST_BROWSER="/usr/bin/chromium"
    TEST_BROWSER="/usr/bin/google-chrome"

    # Uncomment one or the other, depending on which one you want to use.
    #TEST_FILE="test.y4m"
    TEST_FILE="test.mjpeg"

    "$TEST_BROWSER" \
        --user-data-dir="/tmp/chrome-profile" \
        --use-fake-ui-for-media-stream \
        --use-fake-device-for-media-stream \
        --use-file-for-fake-video-capture="$TEST_FILE" \
        --enable-logging=stderr
