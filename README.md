# Arch Linux for the 2013 Chromebook Pixel
The Chromebook Pixel (2013) is an iconic piece of hardware. Since it's debut in 2013 I have dreamed of owning one. This last year the 5 years of support from the mothership Google HQ has stopped. Thus, you can now buy a pixel from retailers for a fraction of the price, but Google has stopped supporting it. To me it seemed like the perfect Linux laptop with its great high quality hardware and solid build. I know that there are other people out there that are hoping to purchase this hardware and run "normal" Linux on it. So I've decided to make this guide/wiki for other people who are fighting the hardware as much as I have.

## Put your Pixel into "dev" mode
These steps are outlined better at the [chromium dev blog](https://www.chromium.org/chromium-os/developer-information-for-chrome-os-devices/chromebook-pixel). But if you are lazy like me, you can just fallow these steps:
1. Hold down the `<ESC> + <REFRESH/F3>` key's and hit power until the keyboard back light comes on.
2. On the bright warning screen it `<CTRL>+<D>`
3. Wait for the system to be wiped. (THIS WILL DELETE ALL USER DATA ON THE DEVICE).
4. Once your Pixel boots, either set it up just enough to get to a browser and hit `<CTRL>+<ALT>+<T>`, OR you can skip setup by pressing `<CTRL>+<ALT>+<FORWARD/F2>` on the setup screen.
5. If you have opted to set up your device a bit you can type `shell` then `sudo bash` to get to a root prompt. If you have not set up your device you can login with the user name of `chronos` and then type the command `sudo bash`. Either way we need to get to a root prompt.
6. Now type in `crossystem dev_boot_usb=1 dev_boot_legacy=1`. This will allow us to boot usb devices through legacy (BIOS) boot.

## Install Arch Linux
You don't have to, but it is my chosen distribution of Linux.

