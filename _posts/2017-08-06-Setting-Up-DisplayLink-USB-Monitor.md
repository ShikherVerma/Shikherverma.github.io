---
layout: post
title: "Setting Up DisplayLink USB Monitor"
subtitle: "Portable usb monitors are the best thing since sliced bread!"
date: 2017-08-06 22:40:45
author: "Shikher Verma"
header-img: "img/posts/monitor-bg.jpg"
comments: true
tags: [ CodeMonkey ]
---

# Setting up Display Link USB monitor on Arch Linux

I recently bought a USB external monitor ( amazon link: [AOC e1759Fwu](https://www.amazon.com/gp/product/B00LPC3U8Q/)). Installing necessary drivers for it was a little non-trivial so I am documenting my installation, just in case someone else encounters the same problem.

## Kernel Headers
Install Linux kernel headers because kernel headers are needed to use DKMS ([Dynamic Kernel Module Support](https://wiki.archlinux.org/index.php/Dynamic_Kernel_Module_Support)).

DKMS allow managing and using kernel modules which are outside the kernel source code. We will add a DKMS kernel module for multiple monitors.

If you haven't changed the kernel that installs with the base arch installation, just install:
```bash
pacman -S linux-headers
```

Skip to next section if the above works. In case you have changed the kernel, find your Linux version:
```bash
uname -r
```
And install the right linux headers from [AUR](https://aur.archlinux.org/). I recommend using [pacaur](https://github.com/rmarquis/pacaur). To install pacaur download and run a script (that I keep in my [dotfiles](https://github.com/ShikherVerma/dotfiles)) by:
```bash
pacman -S --noconfirm --needed curl
bash <(curl -sL https://raw.githubusercontent.com/ShikherVerma/dotfiles/master/dotbot-plugins/bootstrap-pacaur)
```
Search for `linux-headers` packages:
```bash
pacaur -Ss linux headers
```
Install the package that is meant for your linux version by:
```bash
pacaur -S <package name>
```

## evdi kernel module
`evdi` package is a DKMS kernel module that enables management of multiple screens. `evdi` is a dependency of `DisplayLink` package. If you install `DisplayLink` it will automatically install `evdi` as a dependency but nothing would work! That's because you need to install `evdi-pre-release` for the latest kernel ([evdi aur comments](https://aur.archlinux.org/packages/evdi/)). And since arch is a rolling release, you would most probably be on the latest kernel.

Install `evdi-pre-release` with `pacaur -S evdi-pre-release`. Right now the package seems to be broken as it gives an error, saying kernel needing to be configured. This is a bug, which has been fixed in upstream's development branch but is not yet there in the aur package. So you need to modify the package to use the `devel` development branch and manually install the package.
First step in manually installing a aur package is to git clone the aur package's repo (repo = acronym for git repository) . The repo is given on the [evdi-pre-release aur page](https://aur.archlinux.org/packages/evdi-pre-release/). Lets clone it:
```bash
git clone https://aur.archlinux.org/evdi-pre-release.git
```
Change the `PKGBUILD` with `nvim evdi-pre-release/PKGBUILD` to:
```bash
# Maintainer: PlusMinus

_libname=evdi
pkgname=$_libname-pre-release
pkgver=1.4.1
pkgrel=8
pkgdesc="A Linux® kernel module that enables management of multiple screens."
arch=('i686' 'x86_64')
url="https://github.com/DisplayLink/evdi"
license=('GPL')
groups=()
depends=(dkms)
makedepends=()
optdepends=()
provides=("$_libname=$pkgver")
conflicts=($_libname)
backup=()
options=()
install=$pkgname.install
changelog=$pkgname.Changelog
source=($_libname-devel.zip::https://github.com/DisplayLink/evdi/archive/devel.zip)
md5sums=('replace-with-md5sum')
noextract=()

build() {
# We only need to build the library in this step, dkms will build the module
cd "$_libname-devel/library"

make
}

package() {
# Predfine some target folders
SRCDIR="$pkgdir/usr/src/$_libname-$pkgver"	# This one is needed for dkms
LIBNAME=lib$_libname

cd "$_libname-devel"

install -D -m 755 library/$LIBNAME.so $pkgdir/usr/lib/$LIBNAME.so

install -d $SRCDIR
install -D -m 755 module/* $SRCDIR
}
```
Download the `devel.zip` package, calculate its md5sum and put it in the `PKGBUILD` file above. To do all this you can run:
```bash
pacaur -S --noconfirm --needed wget
wget https://github.com/DisplayLink/evdi/archive/devel.zip
md5sum devel.zip
```
Find the text `replace-with-md5sum` in the `PKGBUILD` above and replace it with the md5sum calculate last command above. Now the `evdi-pre-release/PKGBUILD` file is ready. Lets build the package:
```bash
cd evdi-pre-release/
makepkg -sic
```
Check if it installed correctly. Run:
```bash
dkms status
```
it should include a line like this `evdi, 1.4.1, 4.12.4-1-ARCH, x86_64: installed` Here `1.4.1` is the evdi version, `4.12.4-1-ARCH` is the kernel version. These numbers might be different for you. But if you find a link starting with `evdi` it means the module was installed successfully.

## Display Link drivers

Next we need to install the `displaylink` package. Install it with `pacaur -S displaylink`. Unfortunately the `displaylink` package is also broken at the time of writing this. This is due to some web developer adding a terms and conditions page before letting us download the drivers. Luckily one of our fellow awesome arch user has fixed it and shared a [repo](https://github.com/vs0uz4/displaylink_aur). His fix would soon be in the aur. But right now lets clone the repo and build it manually:
```bash
git clone https://github.com/vs0uz4/displaylink_aur displaylink
cd displaylink
makepkg -sic
```
Once installed, we have to make sure we are using `udl` kernel module that manage displaylink driver, its a rewrite of the original `udlfb`, which the current driver use as a base. Activate `udl` kernel module, blacklist `udlfb` and start `displaylink.service`:
```bash
modprobe udl
nvim /etc/modprobe.d/noudlfb.conf
```
Write:
```bash
# Do not load the 'udlfb' module on boot. Using `udl`
blacklist udlfb
```
Configure modprobe to load udl on boot:
```bash
nvim /etc/modprobe.d/udl.conf
```
Write:
```bash
# Support multimonitor
udl
```
enable and start the `displaylink.service`:
```bash
systemctl enable diplaylink.service
systemctl start diplaylink.service
```

Wait for 30 seconds the second monitor should become active (will show AOC splash)

Check `xrandr` output, it should show an `DVI connected` like mine shows:
```bash
Screen 0: minimum 320 x 200, current 2966 x 900, maximum 8192 x 8192
eDP-1 connected primary 1366x768+0+0 (normal left inverted right x axis y axis) 277mm x 156mm
   1366x768      60.00*+  40.00  
   1024x768      60.04    60.00  
   960x720       60.00  
   928x696       60.05  
   896x672       60.01  
   800x600       60.00    60.32    56.25  
   700x525       59.98  
   640x512       60.02  
   640x480       60.00    59.94  
   512x384       60.00  
   400x300       60.32    56.34  
   320x240       60.05  
HDMI-1 disconnected (normal left inverted right x axis y axis)
DVI-I-2-1 connected 1600x900+1366+0 (normal left inverted right x axis y axis) 382mm x 215mm
   1600x900      60.01*+
   1440x900      59.90  
   1280x800      59.91  
   1280x720      60.00  
   1024x768      60.00  
   800x600       60.32    56.25  
   848x480       60.00  
   640x480       59.94  
  1024x768 (0x79) 65.000MHz -HSync -VSync
        h: width  1024 start 1048 end 1184 total 1344 skew    0 clock  48.36KHz
        v: height  768 start  771 end  777 total  806           clock  60.00Hz
  800x600 (0x7e) 40.000MHz +HSync +VSync
        h: width   800 start  840 end  968 total 1056 skew    0 clock  37.88KHz
        v: height  600 start  601 end  605 total  628           clock  60.32Hz
  800x600 (0x7f) 36.000MHz +HSync +VSync
        h: width   800 start  824 end  896 total 1024 skew    0 clock  35.16KHz
        v: height  600 start  601 end  603 total  625           clock  56.25Hz
  640x480 (0x83) 25.175MHz -HSync -VSync
        h: width   640 start  656 end  752 total  800 skew    0 clock  31.47KHz
        v: height  480 start  490 end  492 total  525           clock  59.94Hz
```

Now you can configure the display your self using `xrandr`. But I recommend installing `arandr`. `arandr` is an awesome tool that lets you configure `xrandr` using a very simplistic gui.
Install arandr with `pacaur -S arandr` and start it with `arandr`. Go to output menu, enable the second display, place it where ever you want and apply.

## Intel screen freeze problem ;(
I wish that was the end of story but if you are unlucky like me and using an Intel gpu you will experience screen freeze. For me the screen only updated when I applied the settings in `arandr`. To fix it you need to install `xf86-video-intel` package with `pacaur -S xf86-video-intel` and create a configuration file with `nvim /usr/share/X11/xorg.conf.d/20-intel.conf`  and write:
```bash
Section "Device"
  Identifier "Intel Graphics"
  Driver "Intel"
  Option "AccelMethod" "sna"
  Option "TearFree" "true"
  Option "TripleBuffer" "true"
  Option "MigrationHeuristic" "greedy"
  Option "Tiling" "true"
  Option "Pageflip" "true"
  Option "ExaNoComposite" "false"
  Option "Tiling" "true"
  Option "Pageflip" "true"
EndSection
```
Now if you run `xrandr --listproviders`. It would show 2 providers. Provider 0 would be Intel and provide 1 would be modesetting. Connect provider 1 to provider 0:
```bash
xrandr --setprovideroutputsource 1 0
``` 
Reboot now and the USB monitor should start working normally. Unfortunately screen rotation still doesn't work. DisplayLink's official site has a "Known issues with DisplayLink ubuntu support" page which says "Rotation is not supported due to missing functionality in the generic modesetting driver". I did not try to get rotation working since this is good enough for me. Lack of rotation support is not a deal breaker `┐( ˘_˘ )┌`. See the arch wiki page on [DisplayLink](https://wiki.archlinux.org/index.php/DisplayLink) for more details.

Check out [my dotfiles](https://github.com/shikherverma/dotfiles) for more awesome configurations :)
