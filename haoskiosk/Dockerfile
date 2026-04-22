################################################################################
# Add-on: HAOS Kiosk Display (haoskiosk)
# File: Dockerfile
# Version: 1.3.1
# Copyright Jeff Kosowsky
# Date: April 2026
################################################################################

ARG BUILD_FROM
FROM homeassistant/aarch64-base:latest

ARG BUILD_VERSION
ENV ADDON_VERSION=${BUILD_VERSION}

#===============================================================================
##### Install X, Browser and all necessary dependencies
##### Install X, Browser and all necessary dependencies
RUN apk update && apk add --no-cache \
## Xserver and graphics
     xorg-server \
     chromium \
     xf86-video-modesetting \
     mesa-dri-gallium \
     mesa-egl \
     mesa-gles \
     mesa-va-gallium \
     libva-vdpau-driver \
     libdrm \
### X utils
     xdotool \
     xinput \
     xrandr \
     xset \
     libxkbcommon \
     setxkbmap \
### Input, udev
     xf86-input-libinput \
     libinput \
     udev \
     libinput-udev \
     libevdev \
     evtest \
### Window, mouse, keyboard management
     openbox \
     onboard \
     unclutter-xfixes \
     ttf-dejavu \
### Linux utils
     bash \
     util-linux \
     patch \
     scrot \
### Full Timezone info
     icu-data-full \
### Python libraries
    py3-pip \
    py3-xlib \
### Sound
     pulseaudio-utils \
### VNC (for debugging)
      x11vnc \
### System services
      dbus \
      dbus-x11 \
   && rm -rf /var/cache/apk/*

#===============================================================================

##### Set the display variable
ENV DISPLAY=:0

##### Copy over 'xorg.conf.default' and lua 'userconf.lua' file
COPY xorg.conf.default /etc/X11/
COPY userconf.lua /root/.config/luakit/
COPY translations/*.yaml /translations/

COPY run.sh /
RUN chmod a+x /run.sh

COPY mouse_touch_inputs.py gesture_commands.json /
COPY rest_server.py /
RUN pip install --no-cache-dir  --break-system-packages aiohttp #Required for rest_server.py

#### Patches
# Need to patch 'unique_instance.lua' so that new instance urls overwrite active url rather than add new tab
CMD ["/run.sh"]
