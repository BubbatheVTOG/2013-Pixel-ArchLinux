# Arch Linux for the 2013 Chromebook Pixel
<a href="http://imgur.com/OIyd6iQ"><img src="http://i.imgur.com/OIyd6iQ.jpg" title="Arch Linux on the 2013 Chromebook Pixel" /></a>
The Chromebook Pixel (2013) is an iconic piece of hardware. Since it's debut in 2013 I have dreamed of owning one. This last year Google stopped supporting the 2013 Pixel. Now you can now buy a Pixel from retailers for a fraction of the original price. To me it seemed like the perfect Linux laptop with its great high quality hardware and solid build quality. I know that there are other people out there that are hoping to purchase this hardware and run "normal" Linux on it. So I've decided to make this guide/wiki for other people who are fighting the hardware as much as I have. The 5 year old processor that is in the Pixel unfortunately doesn't have many of the power improvements that have been made in the last few years. With some of the steps listed here you can improve it quite a bit (7-7:30hrs on a full charge) and reduce *some* of the fan noise.

`//TODO:`I will also include copies of my current configuration files in this repository.

## Put your Pixel into "dev" mode
These steps are outlined better at the [chromium dev blog](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/chromebook-pixel). But if you are lazy like me, you can just fallow these steps:
1. Hold down the `<ESC>+<REFRESH/F3>` key's and hit power until the keyboard back light comes on.
2. On the bright warning screen it `<CTRL>+<D>`
3. Wait for the system to be wiped. (THIS WILL DELETE ALL USER DATA ON THE DEVICE).
4. Once your Pixel boots, either set it up just enough to get to a browser and hit `<CTRL>+<ALT>+<T>`, OR you can skip setup by pressing `<CTRL>+<ALT>+<FORWARD/F2>` on the setup screen.
5. If you have opted to set up your device a bit you can type `shell` then `sudo bash` to get to a root prompt. If you have not set up your device you can login with the user name of `chronos` and then type the command `sudo bash`. Either way we need to get to a root prompt.
6. Now type in `crossystem dev_boot_usb=1 dev_boot_legacy=1`. This will allow us to boot usb devices through legacy (BIOS) boot.
7. You can now reboot your pixel and boot to whatever Linux based OS you would like.

## Removing RW Protection
In the current state that you're in by fallowing this guide your Chromebook Pixel will have a nasty habit of "forgetting" about your Linux install if it ever loses power (runs out of battery). If you so choose you can remove the RW protection screw and washer so that you can fully re-write the BIOS of your Pixel to turn it into a "regular" PC. I have removed my RW protection screw, but the binary files hosted at [johnlewis.ie](https://johnlewis.ie/Chromebook-ROMs/) were not available at the time of this writing. There are also some really exciting stuff going on at [Mr. Chromboox's](https://mrchromebox.tech/#home) site with EFI booting, but the 2013 Pixel is marked as "experimental", and further research showed that not only was it [not fully functional](https://www.reddit.com/r/chromeos/comments/5rx4pk/flashing_the_bios_of_the_chromebook_pixel/), but that the 2013 Pixel **IS NOT** fixable via hardware flashing. Because of this I've decided to throw caution to the wind and keep the stock BIOS until I either compile my one myself, or wait for someone more qualified to do a tested and feature rich version. I do plan on flashing one as soon as one seems stable, and will update this wiki after. In the meantime here are the steps that I took for taking apart my Pixel with some pictures.

Glory Shot (I guess?):
<a href="http://imgur.com/yMDpRAl"><img src="http://i.imgur.com/yMDpRAl.jpg" title="2013 Chromebook Pixel torn apart" /></a>

RW Protection Screw:
<a href="http://imgur.com/s6AzHyM"><img src="http://i.imgur.com/s6AzHyM.jpg" title="RW Proteciton Screw" /></a>

### Needed Tools:
1. Size 0 Philips head screw driver.
2. **Suction Cup** (NEEDED!)
3. *mabye a prying tool*

### Steps Taken:
1. Remove the 4 rubber feet.
2. Unscrew the 4 screws under the feet.
3. Suction cup the center of the back panel and pull **HARD**. (This avoids prying and bending the back to get around the clips holding the back panel on.)
4. Locate the RW protection screw on the right side and remove that, and the copper washer.
5. Push the back panel on.
6. Replace the 4 screws and the 4 rubber feet.

I have used Mr. Chrombox's script to remove the blindingly bright "developer mode" warning screen. If you have removed your RW protection screw I'd recommend doing the same (for the sake of your eyes).

## Install Arch Linux
You don't have to, but it is my chosen distribution of Linux.
From here on this guide will be Arch Linux centric, but other Linux's should work just fine.

## Necessary Packages (Arch Linux package names)
These packages are for Arch Linux, but other Linux's should also have them available.
1. `intel-ucode` && `linux-headers` This stops *some* of the stupid fan noise.
2. `acpi` & `acpid` This lets the OS use hardware based events for power managment.
3. `thermald` This reduces *some* of the stupid fan noise.
4. `chromebook_keyboard_backlight_driver` Enables use to tweak the keyboard backlight listed in `/sys/class/leds/chromeos::kbd_backlight/`
5. `cpupower` Lets us set power save mode using cron.
6. `tlp` OR `laptop-mode-tools` (plus optional packages. ie: hdparm, ethtool,etc.) Enables lower power usage when ACPI says we're on on battery.

## Configuration Stuff
This section will contain some descriptions about the changes that I have made to improve battery live and reduce heat.

Most of the configuration that I have done are in two places. The first is just the [Arch Wiki Power Management](https://wiki.archlinux.org/index.php/Power_management). Instead of copy-pasting that, or retyping it you should just go there and do all of the steps listed.

The second most helpful configuration that I did was tossing a bunch of options at the Linux CMDLINE as listed here:
1. `nmi_watchdog=0` Disables watchdog, saves some battery.
2. `resume=/dev/sda1` Resume for suspend-to-disk.
3. `elevator=noop` Changes the I/O scheduler.
4. `i915.enable_fbc=1` Enables the Intel frame buffer compression. This helps reduce load on the iGPU.
5. `i915.semaphores=1` Enables Intel semaphores, which is [this](https://en.wikipedia.org/wiki/Semaphore_(programming)).
6. `i915.lvds_downclock=1` Down clocks the display a bit. Again reducing how much we are using the iGPU.
7. `i915.modeset=1` Forces kernel mode setting. This is probably redundant, but just in case.
8. `i915.i915_enable_rc6=7` Enables **ALL** of the low power states for the iGPU.
9. `pcie_aspm=force` Force enables PCIe "Active State Power Management".
10. `tpm_tis.force=1` Force enables the "Trusted Platform Module" in the kernel. This is needed for suspend and/or hibernation.
11. `tpm_tis.interrupts=0` Disables interrupts from the TPM.
12. `modprob.blacklist=uvcvideo,qmi_wwan` Blacklist the camera and the built-in cellular chip (if applicable). This helps save more power.
13. `intel_iommu=on` Again this is a bit redundant, but it force enables the iGUP. Other options can be read [here](https://www.kernel.org/doc/Documentation/Intel-IOMMU.txt).
14. `iomem=relaxed` ...does something...I added it for a reason, but re-researching this yealed nothing relevant.
15. `acpi_osi=Linux` Tells ACPI that we are on Linux.
16. `acpi=force` Force on ACPI.
17. `acpi_enforce_resources=lax` Enables us to look at some more core temps for the CPU.

Your `/etc/default/grub` should look something like this (this is also easier to copy-paste):
~~~~
GRUB_CMDLINE_LINUX_DEFAULT="quiet nmi_watchdog=0 resume=/dev/sda1 elevator=noop i915.enable_fbc=1 i915.semaphores=1 i915.lvds_downclock=1 i915.modeset=1 i915.i915_enable_rc6=7 pcie_aspm=force tpm_tis.force=1 tpm_tis.interrupts=0 modprobe.blacklist=uvcvideo,qmi_wwan intel_iommu=on iomem=relaxed acpi_osi=Linux acpi=force acpi_enforce_resources=lax"
~~~~
