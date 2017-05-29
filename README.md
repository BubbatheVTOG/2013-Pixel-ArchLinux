# Arch Linux for the 2013 Chromebook Pixel
The Chromebook Pixel (2013) is an iconic piece of hardware. Since it's debut in 2013 I have dreamed of owning one. This last year the 5 years of support from the mothership, Google HQ has stopped. Now you can now buy a Pixel from retailers for a fraction of the price. To me it seemed like the perfect Linux laptop with its great high quality hardware and solid build quality. I know that there are other people out there that are hoping to purchase this hardware and run "normal" Linux on it. So I've decided to make this guide/wiki for other people who are fighting the hardware as much as I have. I will also include copies of my current configuration files in this repository.

## Put your Pixel into "dev" mode
These steps are outlined better at the [chromium dev blog](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/chromebook-pixel). But if you are lazy like me, you can just fallow these steps:
1. Hold down the `<ESC> + <REFRESH/F3>` key's and hit power until the keyboard back light comes on.
2. On the bright warning screen it `<CTRL>+<D>`
3. Wait for the system to be wiped. (THIS WILL DELETE ALL USER DATA ON THE DEVICE).
4. Once your Pixel boots, either set it up just enough to get to a browser and hit `<CTRL>+<ALT>+<T>`, OR you can skip setup by pressing `<CTRL>+<ALT>+<FORWARD/F2>` on the setup screen.
5. If you have opted to set up your device a bit you can type `shell` then `sudo bash` to get to a root prompt. If you have not set up your device you can login with the user name of `chronos` and then type the command `sudo bash`. Either way we need to get to a root prompt.
6. Now type in `crossystem dev_boot_usb=1 dev_boot_legacy=1`. This will allow us to boot usb devices through legacy (BIOS) boot.
7. You can now reboot your pixel and boot whatever Linux based OS you would like.

## Removing RW Protection

In the current state that you're in by fallowing this guide your Chromebook Pixel will have a nasty habit of "forgetting" about your Linux install if it ever loses power (runs out of battery). If you so choose you can remove the RW protection screw and washer so that you can fully re-write the BIOS of your Pixel to turn it into a "regular" PC. I have done this, but the binary files hosted at [johnlewis.ie](https://johnlewis.ie/Chromebook-ROMs/) were not available at the time of this writing. There is some really exciting stuff going on at [Mr. Chromboox's](https://mrchromebox.tech/#home) site with EFI booting, but the 2013 Pixel is marked as "experimental", and further research showed that not only was it [not fully functional](https://www.reddit.com/r/chromeos/comments/5rx4pk/flashing_the_bios_of_the_chromebook_pixel/), but that the 2013 Pixel **IS NOT** fixable via hardware flashing. Because of this I've decided to throw caution to the wind and keep the stock BIOS until I either compile my one myself, or wait for someone more qualified to do a tested and feature rich version. I do plan on flashing one as soon as one seems stable, and will update this wiki after. In the meantime here are the steps that I took for taking apart my Pixel with some pictures.

Glory Shot (I guess):
![Alt](://photos.google.com/share/AF1QipPCzF3pFfRhCuRpL_67cpD_WdHvOAkEDVPVUyAyXHJuWc1xwvm1qD5CfnvXrlngIw?key=bk0xRWZOVGlNaWp0WUt5X1oxb0hCaTkxRDhadHJn "2013 Chromebook Pixel Full-Open")

RW Protection Screw:
![Alt](https://photos.google.com/share/AF1QipPHTb21KiQx96llhPrZvjrc6jLkBtAUVjjCt2GaLZW-uOsBqtvkKW_OKilmFR5DvA?key=V3JmZjd4SzJ6blNQVXYyRW85TEtFU1NNTm92bkpn "2013 Chromebook Pixel RW Protection Screw")

### Steps Taken:
1.

I have used Mr. Chrombox's script to remove the blindingly bright "developer mode" warning screen. If you have removed your RW protection screw I'd recommend doing the same (for the sake of your eyes).

## Install Arch Linux
You don't have to, but it is my chosen distribution of Linux.
From here on this guide will be Arch Linux centric, but other Linux's should work just fine.

## Necessary Packages (Arch Linux package names)
Most of these packages are for Arch Linux, but other Linux's also have them available.
1. `intel-ucode` This stops *some* of the stupid fan noise.
2. `acpi` & `acpid` This lets the OS use hardware based events for power managment.
3. `thermald` This reduces *some* of the stupid fan noise.
4. `chromebook_keyboard_backlight_driver` Enables use to tweak the keyboard backlight listed in `/sys/class/leds/chromeos::kbd_backlight/`
5. `cpupower` Lets us set powersave mode using cron.
6. `tlp` OR `laptop-mode-tools` (plus optional packages. ie: hdparm, ethtool,etc.) Enables lower power usage when acpi says we're on on battery.

## Configuration Stuff



