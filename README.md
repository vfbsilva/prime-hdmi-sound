Solution:
create
/etc/systemd/system/fix-hdmi-audio.service
1. [Unit]
2. Description=nVidia HDMI Audio Fixer
3. Before=systemd-logind.service display-manager.service
4. After=module-init-tools.service
5. [Service]
6. Type=oneshot
7. ExecStart=/root/fix-hdmi-audio.sh
8. [Install]
9. WantedBy=multi-user.target

and
/root/fix-hdmi-audio.sh
1. #!/bin/sh
2. setpci -s 01:00.0 0x488.l=0x2000000:0x2000000
3. rmmod nvidia-uvm nvidia-drm nvidia-modeset nvidia
4. sh -c 'echo 1 > /sys/bus/pci/devices/0000:01:00.0/remove'
5. sh -c 'echo 1 > /sys/bus/pci/devices/0000:00:01.0/rescan'
6. modprobe nvidia nvidia-modeset nvidia-drm nvidia-uvm

And then execute the following commands (as root) to make it do this at boot:

1. chmod +x /root/fix-hdmi-audio.sh
2. systemctl enable fix-hdmi-audio.service

After each reboot, I now see the HDMI audio device on my laptop
