# Configure Fedora to not crash on HP Z2 Mini Workstation with FlexIO board installed

This is very specific situation and might not be useful to other users of HP Z2 Mini Workstation. But if you observe same behavior then this might help you.

I bought Flex IO v3 board with two USB-C ports. Installation was easy and after booting into my Fedora, both ports worked without any issues. Issue started later when suddenly computer froze. Nothing helped, not even REISUB. So, I pushed power button and waited util PC shuts down completely. Then pushed again to boot it up and - boom! PC does not want to boot and power button's LED blinks in specific pattern. 3 Red and 4 white. Quick searching revealed that the error - "Power failure on the system board". I though something is wrong with FlexIO board itself or PSU is not powerful enough. 

Interesting was that PC never crashed when I used ports. I put two USB sticks and run test on them for almost 20 minutes, no crashes! But as soon as I leave ports alone and do some other stuff, PC will crash eventually.

After checking my configuration I found that I have maxed out PSU for this unit! So next step was to Google my problem again. And only after that I found solution!

**Aggressive Power Management** of Fedora. This exerp is from internet:


> "Linux (Fedora 44) uses aggressive power management (TLP, powertop, or kernel-level PCIe ASPM) to suspend unused PCIe devices. When the OS tries to wake up or suspend the FlexIO card's PCIe controller, it can cause a sudden transient current spike that triggers the motherboard's over-current protection (OCP)."

Solution to fix this was to disable PCIe ASPM (a.k.a. Active State Power Management) by GRUB configuration. To do so, edit GRUB config

```bash
sudo nano /etc/default/grub # or another other text editor
```


Find the line starting with `GRUB_CMDLINE_LINUX` and add `pcie_aspm=off` inside the quotes.

```
GRUB_CMDLINE_LINUX="... pcie_aspm=off"
```

Save file and update GRUB

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

Reboot the system and everything will work!

How disabling PCIe ASPM will affect the system? Since Z2 mini is not a laptop but desktop, the impact is negligible. What it actually does? ASPM is designed to save power by putting PCIe lanes into a low-power state when they aren't transmitting data. By turning it off (pcie_aspm=off), you are forcing the PCIe buses to stay fully awake and active all the time.

Power consumption could increase like 1 - 3 Watts of total draw when idle. I did not confirm this myself. I am happy that it is working and if it is additional 3 Watts during idle, then I am ok with it. Google also tells that temperatures could potentially be higher, but I only monitor CPU temp, and it is the same as it was before.

If you know any other affects, let me know :)
