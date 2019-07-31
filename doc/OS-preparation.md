# OS Image Preparation

* Download the 2019-06-19-ubiquity-xenial-lxde OS image from [Ubiquity Robotics](https://downloads.ubiquityrobotics.com/pi.html) and use your preferred tool to burn it into a micro SD card.
* Insert the micro-SD card into the Raspberry Pi and turn it on. It will take several minutes for the first boot to complete some configurations.
* Once the Raspberry has booted, you should see a WiFi network from your laptop with name **ubiquityrobotXXXX** where **XXXX** are the last 4 characters of the Raspi's MAC address. The password of the network is **robotseverywhere**.
* Ssh into the Raspi with username and password **ubuntu/ubuntu**
```
ssh ubuntu@10.42.0.1
```
* Disable Ubiquity's magni-base service and reboot.
```
sudo systemctl disable magni-base
sudo reboot
```
* Once the Raspi reboots, reconnect to its WiFi network and open an SSH session with `ssh ubuntu@10.42.0.1`.
* Connect the Raspi to the Internet using the Ethernet port.
* Upgrade the Operating System
```
sudo apt update
sudo apt upgrade
```
* Enable the Raspberry camera
```
sudo raspi-config

# Navigate to "3 Interfacing Options" -> "P1 Camera" and enable the camera
# Navitate to "5 Advanced Options" -> "A3 Memory Split" and set the GPU memory to 256.
# Once finished, the config app will ask for reboot the system.
```
* Install some base packages
```
sudo apt install -y \
    tmux \
    ros-kinetic-image-view \
    ros-kinetic-web-video-server \
    libgstreamer1.0-0 \
    gstreamer1.0-plugins-base \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-libav \
    gstreamer1.0-doc \
    gstreamer1.0-tools \
    gstreamer1.0-x \
    gstreamer1.0-alsa \
    gstreamer1.0-pulseaudio
```
* Fix the locale of the system:
```
sudo locale-gen en_AU.UTF-8
sudo locale-gen en_US.UTF-8
sudo update-locale LANG=en_US.UTF-8
```
* If the last command not works finem, use instead:
```
export LC_ALL="en_US.UTF-8"
export LC_CTYPE="en_US.UTF-8"
sudo dpkg-reconfigure locales
```
* Python 2 packages
```
sudo pip2 install --upgrade pip
sudo pip2 install --upgrade \
    jupyterlab \
    matplotlib \
    adafruit-pca9685
```
* Configure JupyterLab
```
# Generate a configuration file
jupyter-lab --generate-config
jupyter notebook password
# set a password, for ease, I use ubuntu

# This generates the file at /home/ubuntu/.jupyter/jupyter_notebook_config.py
```
* Open the configuration file using `nano /home/ubuntu/.jupyter/jupyter_notebook_config.py` and uncomment and edit the following options:
```
c.NotebookApp.allow_origin = '*'

# Listen over the WiFi adapter, for JupyterLab on Python2, using '*' does not work
c.NotebookApp.ip = '10.42.0.1'

c.NotebookApp.open_browser = False

# This sha1 hash is for password: ubuntu
c.NotebookApp.password = u'sha1:3ce04477235c:9829c5b5775eb68a50127392a4b7c23757bf67c6'
```
* Start the **jupyterlab** server with `jupyter lab`. You should be able to access the server from your Laptop using http://10.42.0.1:8888 and typing *ubuntu* as password.
* To make **jupyterlab** server available automatically from boot up, create a systemctl service to configure its launch.
```
# create the service file
sudo touch /etc/systemd/system/jupyter.service
sudo chmod 644 /etc/systemd/system/jupyter.service
```
* Open an editor with `sudo nano /etc/systemd/system/jupyter.service` and copy/paste the following text:
```
[Unit]
Description=Jupyter Notebook

[Service]
Type=simple
ExecStart=/usr/local/bin/jupyter lab
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu
Restart=always
RestartSec=10
#KillMode=mixed

[Install]
WantedBy=multi-user.target
```
* The service can be controlled manually with the following commands:
```
sudo systemctl start jupyter.service 
sudo systemctl stop jupyter.service 
sudo systemctl status jupyter.service 
```
* Enable the service upon system boot
```
sudo systemctl enable jupyter.service 
```
* Restart the Raspi. Once the WiFi starts, type the URL of the jupyter server http://10.42.0.1:8888 on your browser. You should be prompted with the password dialog and after that with the Jupyter launcher screen.

**TODO: put screenshot here**
