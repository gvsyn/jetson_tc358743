# README is WiP

Still sorting through the notes on this - with decent install instructions (actual kernel package pending, or DKMS or similar)


`mkdir building && cd building`

```git clone https://github.com/gvsyn/jetson_tc358743
mkdir building && cd building
wget  https://developer.nvidia.com/embedded/l4t/r32_release_v7.1/sources/t210/public_sources.tbz2
tar -xf public_sources.tbz2  Linux_for_Tegra/source/public/kernel_src.tbz2 --strip-components=3
tar xf kernel_src.tbz2
cp ~/jetson_tc358743/* ./ -r
cd kernel/kernel-4.9
cp /proc/config.gz . && gunzip config.gz && mv config .config
make menuconfig

Device Drivers > Multimedia Support > I2C Encoders, decoders, sensors and other helper chips
Toshiba TC358743 decoder should be * (press y or space on the option)

grep -i tc358743 .config #checking if its really enabled  
make -j4
make modules_install
make install
cp arch/arm64/boot/dts/tegra210-p3448-* /boot/

cd /boot
sudo ln -s vmlinuz-4.9.253 vmlinuz
sudo vim extlinux/extlinux.conf

sudo /opt/nvidia/jetson-io/config-by-function.py -o dtbo i2s4
```
### amend the following:
     LINUX /boot/Image
### to
     LINUX /boot/vmlinuz-4.9.253

REBOOT

### THIS COMMAND IS CRITICAL. makes the jetson listen.
`amixer -c tegrasndt210ref cset name='I2S4 codec master mode' 'cbm-cfm'`

