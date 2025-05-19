# Artillery Sidewinder X4 Plus S1 software updating guide
Guide for updating software on the X4 Plus (may apply to other Sidewinders)

The software that comes with the Artillery Sidewinder X4 Plus S1 is fairly outdated. With the latest updates from Artillery, mine was running Armbian Buster, Klipper 0.9, and other outdated software

It's also using the old klipper_config dir, and I'd like to update it to use the more modern printer_data structure.

My goal is to updated to Armbian Bullseye or Bookworm as well as the latest versions of Klipper, Mainsail, Fluidd, and others.

# OS

Update /etc/sources.list

```
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
deb http://deb.debian.org/debian bullseye-backports main contrib non-free
deb http://security.debian.org/ bullseye-security/updates main contrib non-free
```

If you want to update to bookworm, just replace "bullseye" with "bookworm". 

Then update apt and install new packages:

* `sudo apt update`
* `sudo apt dist-upgrade`

# fluidd

Fluidd does not seem to be have any changes made by Artillery, so this easily updates using the moonraker updater.

# mjpg-streamer

Some things can be updated with KIAUH, but it seems like mjpg-streamer has been removed as an option for installing, possibly because the main fork doesn't seem to work on Armbian Bullseye

This is a working repo for mjpg-streamer on Armbian Bullseye and Bookworm

https://github.com/ArduCAM/mjpg-streamer

I basically copied what ~/kiauh/scripts/mjpg-streamer.sh did

I built mjpg-streamer-experimental then copied to the mjpg-streamer directory

* cd mjpg-streamer/mjpg-stramer-experimental
* make
* mv * ../

I created the web folder:

* mkdir ~/mjpg-streamer/www-mjpgstreamer

```
  cat <<EOT >> ~/mjpg-streamer/www-mjpgstreamer/index.html
  <html>
  <head><title>mjpg_streamer test page</title></head>
  <body>
  <h1>Snapshot</h1>
  <p>Refresh the page to refresh the snapshot</p> 
  <img src="./?action=snapshot" alt="Snapshot">
  <h1>Stream</h1>
  <img src="./?action=stream" alt="Stream">
  </body>
  </html>
  EOT
```

Then I updated the config_dir line in `/usr/local/bin/webcamd` and set it to `config_dir=/home/mks/printer_data/config/`

Then I moved klipper_config/webcam.txt to printer_data/config/webcam.txt


# Random things I've found but don't know what to do with

The TFT is completely separate from the rest of this. It looks like there's a running service called `mksclient` in `/home/mks/Desktop/myfile/ws/build` which seems to be how it communicates with the TFT. And a nice, empty src/ directory of course. Seems like it's software from makerbase but nothing on their github seems related.

# printer Info

* CPU: ARM Cortex A53
* MCU: stm32f401xc
* Disk space: 8gb
* Memory: 1gb
