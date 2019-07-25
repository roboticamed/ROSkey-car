# This works
raspivid -t 0 -w 1080 -h 720 -fps 30 -hf -b 2000000 -o - | \
gst-launch-1.0 -e -vvvv fdsrc ! \
h264parse ! \
rtph264pay config-interval=5 pt=96 ! \
udpsink host=10.42.0.234 port=5000


# This works to receive the video on Linux
gst-launch-1.0 -e -v udpsrc port=5000 ! application/x-rtp, payload=96 ! rtpjitterbuffer ! rtph264depay ! avdec_h264 ! xvimagesink sync=false



gst-launch-1.0 -v rpicamsrc num-buffers=-1 ! video/x-raw,width=640,height=480, framerate=41/1 ! timeoverlay time-mode="buffer-time" ! jpegenc !  rtpjpegpay !  udpsink host=10.42.0.234 port=5200
