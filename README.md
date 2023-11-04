#!/bin/bash

############################################################################################################################
##              FUNCTION                  																					                                                           ##
############################################################################################################################
# SU -->  sudo usermod -aG sudo udjin

#=============  DOWNLOAD FILE FROM GOOGLE DISK  =========================================================================================================
gdrive_download () {

CONFIRM=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate "https://docs.google.com/uc?export=download&id=$1" -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')
wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$CONFIRM&id=$1" -O $2
rm -rf /tmp/cookies.txt
}

#=============  INSTALL THEMES XFCE =====================================================================================================================
inst_xfce_themes () {

mkdir /tmp
mkdir /tmp/install
cd /tmp/install
sudo apt-get install psmisc

sudo apt-get install oxygen-icon-theme
xfconf-query -c xsettings -p /Net/IconThemeName -s 'oxygen'
xfconf-query -c xsettings -p /Net/ThemeName -s 'Adwaita-dark'


export FILEID=1fELRFssIxCIXdOAiZpc7HJ4htsuxGHQi
export FILENAME=xfce4-panel.zip
gdrive_download $FILEID $FILENAME
7z x $FILENAME

export FILEID=1IxBFGO-_VEyLHnNYHECDeyMsNdMbzhDB
export FILENAME=win-themes.zip
gdrive_download $FILEID $FILENAME
7z x $FILENAME
sudo cp -axr /tmp/install/win-themes/. /usr/share/themes
xfconf-query -c xfwm4 -p /general/theme -s 'Fugitive-xfce-rounded'

export FILEID=1iP-SNwDvj6lZ3uM5b9c_7oCBlq8rgcAt
export FILENAME=backgrounds.zip
gdrive_download $FILEID $FILENAME
7z x $FILENAME
sudo cp -axr /tmp/install/backgrounds/. /usr/share/backgrounds
xfconf-query -c xfce4-desktop -p /backdrop/screen0/monitor0/workspace0/last-image -s '/usr/share/backgrounds/mountains-wallpapers-1920x1080-00019.jpg'

wget https://s3.amazonaws.com/plugable/bin/fw-0a5c_21e8.hcd
sudo mkdir /lib/firmware/brcm
sudo mv fw-0a5c_21e8.hcd /lib/firmware/brcm/BCM-0b05-17cb.hcd
#dmesg | grep firmware



#rm -rf ~/.config/xfce4/panel
#mkdir ~/.config/xfce4/panel
#rm -f ~/.config/xfce4/xfconf/xfce-perchannel-xml/xfce4-panel.xml
#cp -axr /tmp/install/xfce4/panel/. ~/.config/xfce4/panel

#rm -rf ~/.config/xfce4/xfce-perchannel-xml
#mkdir ~/.config/xfce4/xfce-perchannel-xml
#cp -axr /tmp/install/xfce4/xfconf/xfce-perchannel-xml/. ~/.config/xfce4/xfconf/xfce-perchannel-xml


sudo chmod u+s /usr/sbin/hddtemp 
#killall xfconfd
#xfce4-panel -r

}

#=============  INSTALL Google-Earth   ==================================================================================================================
inst_google_earth () {
cd /tmp/install
wget https://dl.google.com/dl/earth/client/current/google-earth-pro-stable_current_amd64.deb
sudo dpkg -i google-earth-pro-stable_current_amd64.deb
}

#=============  INSTALL  ================================================================================================================================
inst_google_chrome () {
cd /tmp/install
#sudo nano /etc/apt/sources.list.d/google-chrome.list
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
sudo apt-get update
sudo apt-get install google-chrome-stable
}

#=============  INSTALL SMPlayer   ======================================================================================================================
inst_smplayer () {
cd /tmp/install
export FILEID=1s28Ux4X63yta0uFxY7p3KwXF2u-_9Ht2
export FILENAME=smplayer_19.5.0_amd64.deb
gdrive_download $FILEID $FILENAME
sudo dpkg -i $FILENAME
sudo mkdir /home/user/.config/smplayer
sudo cp -r /tmp/install/THEMES/.config/smplayer/. /home/user/.config/smplayer
}

#=============  INSTALL Viber  ==========================================================================================================================
inst_viber () {
cd /tmp/install
wget http://download.cdn.viber.com/cdn/desktop/Linux/viber.deb
sudo dpkg -i viber.deb
sudo apt-get -f install
sudo dpkg -i viber.deb
}

#=============  INSTALL Telegtam  ======================================================================================================================
inst_telegtam () {
sudo add-apt-repository "deb http://ftp.de.debian.org/debian sid main"
sudo apt-get install -y telegram-desktop
}

#=============  INSTALL GIS-WEATHER  ====================================================================================================================
inst_gis_weather () {

echo " +++++++++++ INSTALL GIS-WEATHER ++++++++++++++++ "

cd /tmp/install
export FILEID=1sdSjcuVPGI6xwEue_39i4iRVqUc4ctpJ
export FILENAME=gis-weather_0.8.4.1_all.deb
gdrive_download $FILEID $FILENAME
sudo dpkg -i $FILENAME
# ---------------- seting config files ----------------
cd /tmp/install
export FILEID=1Nz_BCc9hdwg-RxWcdcG4t3BHzj4_ONwW
export FILENAME=conf_gisweather.zip
gdrive_download $FILEID $FILENAME
7z x $FILENAME
cp -axr /tmp/install/conf_gisweather/. /home/$USER
}
#=============  INSTALL Doublecmd  ======================================================================================================================
inst_doublecmd () {
sudo add-apt-repository ppa:alexx2000/doublecmd
sudo apt-get update
sudo apt-get install doublecmd-gtk

}

#=============  INSTALL TiemViewer  =====================================================================================================================
inst_teamviewer () {
wget https://download.teamviewer.com/download/linux/teamviewer_amd64.deb
sudo dpkg -i teamviewer_amd64.deb
sudo apt-get -f install -y
sudo dpkg -i teamviewer_amd64.deb
}

#=============  INSTALL AnyDesk  =======================================================================================================================
inst_anydesk () {
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | apt-key add -
echo "deb http://deb.anydesk.com/ all main" > /etc/apt/sources.list.d/anydesk-stable.list
sudo apt-get install anydesk
 }


#=============  INSTALL SNADP  =======================================================================================================================
inst_snadp () {
sudo apt install snapd
sudo snap install core
sudo snap install snap-store
#sudo systemctl status snapd
sudo systemctl start snapd
sudo ln -s /etc/profile.d/apps-bin-path.sh /etc/X11/Xsession.d/99snap
sudo ln -s /var/lib/snapd/snap /snap

#sudo nano /etc/login.defs
#need to change line
#NV_PATH PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin

 }

#=============  INSTALL Audacious  ======================================================================================================================

inst_audacious () { echo " +++++++++++ INSTALL Audacious ++++++++++++++++ "
sudo apt-get install audacious audacious-plugins -y

# ---------------- seting config files ----------------
cd /tmp/install
export FILEID=1L0-nfgJwHL2Z0h1c5HrCkwNeUixIvTKe
export FILENAME=conf_audacious.zip	
gdrive_download $FILEID $FILENAME
7z x $FILENAME
cp -axr /tmp/install/conf_audacious/. /home/$USER 
}
#########################################################################################################################################################
sudo apt-get update -y
sudo apt-get install wget -y
sudo apt-get install mc -y
sudo apt-get install plank -y
sudo apt-get install guake -y
sudo apt-get install gpart -y
sudo apt-get install vlc -y
sudo apt-get install remmina -y
sudo apt-get install qbittorrent -y

inst_xfce_themes

inst_google_earth
inst_google_chrome
inst_gis_weather
inst_telegtam
inst_audacious
inst_anydesk
inst_snadp

#inst_smplayer
#inst_viber
#inst_doublecmd
#inst_teamviewer









#THE ONLY WAY I could find to TO INSTALL WINEASIO on Debian / Ubuntu
# https://forum.winehq.org/viewtopic.php?t=31961


#sudo apt-get install apt-transport-https gpgv
#sudo dpkg --purge kxstudio-repos-gcc5
#wget https://launchpad.net/~kxstudio-debian/+archive/kxstudio/+files/kxstudio-repos_10.0.3_all.deb
#sudo dpkg -i kxstudio-repos_10.0.3_all.deb
#sudo apt-get update

#sudo apt-get install jackd
#sudo apt-get install wineasio



#sudo apt-get install apt-transport-https software-properties-common
#sudo dpkg --add-architecture i386 
#wget -qO - https://dl.winehq.org/wine-builds/winehq.key | sudo apt-key add -
#sudo apt-add-repository https://dl.winehq.org/wine-builds/debian/
#sudo apt update

#wget -O- -q https://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10/Release.key | sudo apt-key add -    
#echo "deb http://download.opensuse.org/repositories/Emulators:/Wine:/Debian/Debian_10 ./" | sudo tee /etc/apt/sources.list.d/wine-obs.list
#sudo apt update

#sudo apt-get install wine-stable-i386=3.*
#sudo apt-get install wine-stable=3.*
#sudo apt-get install winehq-stable=3.*

#wine regsvr32 /usr/lib/i386-linux-gnu/wine/wine/wineasio.dll.so 
#wine64 regsvr32 /usr/lib/x86_64-linux-gnu/wine/wineasio.dll.so 
#alsamixer

#sudo mkdir /mnt/iso
#sudo mount -o loop /media/user/2ed780e7-a1d4-4cc8-b057-9eca6582db88/1_INSTALL/Windows/Guitar/GuitarRig/Native.Instruments.Guitar\ Rig\ 4.1\ Pro/GR4.iso /mnt/iso

########################################################################################################################################
#sudo apt install wineasio-amd64
#sudo apt install wineasio-i386
#wine64 regsvr32 wineasio.dll
#wine regsvr32 wineasio.dll

#Worked ASIO DRIVERS version
#dpkg-query -l
#ii  wine-rt               2:3.0.2-1~kxstudio3v5        amd64        Microsoft Windows Compatibility Layer (Binary Emulator and Library)
#ii  wine-rt-amd64         2:3.0.2-1~kxstudio3v5        amd64        Microsoft Windows Compatibility Layer (64-bit support)
#ii  wine-rt-i386:i386     2:3.0.2-1~kxstudio3v5        i386         Microsoft Windows Compatibility Layer (32-bit support)
#ii  wine-stable           3.0.5~buster                 amd64        WINE Is Not An Emulator - runs MS Windows programs
#ii  wine-stable-amd64     3.0.5~buster                 amd64        WINE Is Not An Emulator - runs MS Windows programs
#ii  wine-stable-i386:i386 3.0.5~buster                 i386         WINE Is Not An Emulator - runs MS Windows programs
#ii  wineasio              0.9.0+git20110613-2kxstudio1 amd64        Wine ASIO driver for JACK
#ii  wineasio-amd64        0.9.0+git20110613-2kxstudio4 amd64        Wine ASIO driver for JACK (64bit)
#ii  wineasio-i386:i386    0.9.0+git20110613-2kxstudio4 i386         Wine ASIO driver for JACK (32bit)


#ii  jackd                    5+nmu1                           all          JACK Audio Connection Kit (default server package)
#ii  jackd2                   2:1.9.12+git20190321~kxstudio1v5 amd64        JACK Audio Connection Kit (server and example clients)
#ii  jackd2-firewire          2:1.9.12+git20190321~kxstudio1v5 amd64        JACK Audio Connection Kit (FFADO and FreeBoB backends)
#ii  libjack-jackd2-0:amd64   2:1.9.12+git20190321~kxstudio1v5 amd64        JACK Audio Connection Kit (libraries)
#ii  libjack-jackd2-0:i386    2:1.9.12+git20190321~kxstudio1v5 i386         JACK Audio Connection Kit (libraries)
#ii  libjack-jackd2-dev:amd64 2:1.9.12+git20190321~kxstudio1v5 amd64        JACK Audio Connection Kit (development files)
#ii  qjackctl                 2:0.4.5-1kxstudio1v5             amd64        User interface for controlling the JACK sound server

