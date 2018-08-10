# VirtualBox에 RaspberryPI Install.

1. VirtualBox install
2. RaspberryPI install ex)2018-06-27-rpd-x86-stretch.history


## Virtualbox에 install.
1. 새로만들기
2. 이름 : RaspberryPI
   종류 : Linux
   버전 : Ubuntu 3232bit

3. 메모리 2048
4. 지금 새 가상 하드 디스크 만들기
5. VDI 선택
6. 동적 할당
7. 용량은 대충 32GB 정도

## Virtualbox settings
1. 시스템 - 프로세서 - 2개정도 ..
2. 저장소 : cd (iso파일)

## execute raspberry
1. Graphical install
2. Keyboard - Korean
3. Guided - use entire disk
4. SCSI (0,0,0)(sda) - 34.4 GB ATA VBOX HARDDISK
5. All files in one partition (recommended for new users)
6. Finish partitioning and Write changes to disk
7. Partition disks - Yes
8. Install the GRUB boot loader on a hard disk - Yes
9. 두번째 메뉴
10. Finish the installation

### apt setting
```
$ sudo apt-get update
$ sudo apt-get upagrade
$ sudo apt-get install fonts-unfonts-core // 한글 설정

```

### 해상도 설정
* 안돼 ㅡㅡ
```
$ cp /media/cdrom0/* /home/pi/Downloads
$ cd /home/pi/Downloads
$ sudo ./VBoxLinuxAdditions.run
$ vi /etc/X11/xorg.conf

Section "Device"
    Identifier  "Configured Video Device"
    Driver      "vboxvideo"
EndSection

Section "Monitor"
    Identifier  "Configured Monitor"
EndSection

Section "Screen"
    Identifier  "Default Screen"
    Monitor     "Configured Monitor"
    Device      "Configured Video Device"
EndSection
$ sudo reboot
```
