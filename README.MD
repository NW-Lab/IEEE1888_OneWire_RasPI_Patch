RasPIで多点温度計測をするためのPatch
linux/arch/arm/boot/dts/overlays/w1-gpio-overlay.dtsを変更します。

有効にするためには/boot/config.txtの変更が必要です。

(OSの更新)
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade

(ファームウエア更新)
sudo rpi-update
sudo reboot

カーネルの更新
(ルートディレクトリへ)
cd

(gitのインストール)
sudo apt-get install git

(linuxのソースを持ってくる)
git clone --depth=1 https://github.com/raspberrypi/linux

(bcパッケージのインストール)
sudo apt-get install bc

(linux/arch/arm/boot/dts/overlays/w1-gpio-overlay.dtsの変更）
このPatch

(kernel Build Configuration)
cd linux/
KERNEL=kernel7
make bcm2709_defconfig

(kernel Build)
make -j4 zImage modules dtbs
sudo make modules_install
sudo cp arch/arm/boot/dts/*.dtb /boot/
sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
sudo scripts/mkknlimg  arch/arm/boot/zImage /boot/$KERNEL.img
(再起動)
sudo reboot

(configure)
sudo raspi-config
 5 Interface Options  P7 1-Wire  : Enable

sudo vi /etc/modules 以下を追加
　w1-gpio
　w1-therm

sudo vi /boot/config.txt　以下を追加
#dtoverlay=w1-gpio,gpiopin0=4
dtoverlay=w1-gpio,gpiopin0=4

#dtoverlay=w1-gpio,gpiopin1=5
dtoverlay=w1-gpio,gpiopin1=-1

#dtoverlay=w1-gpio,gpiopin2=6
dtoverlay=w1-gpio,gpiopin2=-1

#dtoverlay=w1-gpio,gpiopin3=12
dtoverlay=w1-gpio,gpiopin3=-1

#dtoverlay=w1-gpio,gpiopin4=13
dtoverlay=w1-gpio,gpiopin4=-1

#dtoverlay=w1-gpio,gpiopin5=16
dtoverlay=w1-gpio,gpiopin5=-1

dtoverlay=w1-gpio,gpiopin6=17
#dtoverlay=w1-gpio,gpiopin6=-1

#dtoverlay=w1-gpio,gpiopin7=18
dtoverlay=w1-gpio,gpiopin7=-1

#dtoverlay=w1-gpio,gpiopin8=19
dtoverlay=w1-gpio,gpiopin8=-1

#dtoverlay=w1-gpio,gpiopin9=22
dtoverlay=w1-gpio,gpiopin9=-1

#dtoverlay=w1-gpio,gpiopin10=23
dtoverlay=w1-gpio,gpiopin10=-1

#dtoverlay=w1-gpio,gpiopin11=24
dtoverlay=w1-gpio,gpiopin11=-1

#dtoverlay=w1-gpio,gpiopin12=25
dtoverlay=w1-gpio,gpiopin12=-1

#dtoverlay=w1-gpio,gpiopin13=27
dtoverlay=w1-gpio,gpiopin13=-1

dtdebug=on

dtdebug=on は再起動後　sudo vcdbg log msg で確認後　削除
有効にするポートは-1でない方に変更。

確認
ls /sys/bus/w1/devices/



