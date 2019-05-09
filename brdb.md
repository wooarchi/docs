# DRBD 테스트

## 대상 VM
- drbd01 (centos 7.5)
  - 10.10.10.7
- drbd02 (centos 7.5)
  - 10.10.10.5
  
### elrepo 저장소 등록
```
$ rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
$ rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
```

### drbd 패키지 설치
```
$ yum install drbd84-utils kmod-drbd84
```

### selinux disable
```
$ vi /etc/sysconfig/selunux
SELINUX=diabled
```

### hosts 설정
```
$ vi /etc/hosts
10.10.10.7 drbd01
10.10.10.5 drbd02
```

### disk 확인
```
$ fdisk -l
Disk /dev/vda: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000123f5

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048    41929649    20963801   83  Linux

Disk /dev/vdb: 21.5 GB, 21474836480 bytes, 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

### 파티션 생성
```
fdisk /dev/vda
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): p
Partition number (2-4, default 2): 2
First sector (41929650-41943039, default 41930752): 41930752
Last sector, +sectors or +size{K,M,G} (41930752-41943039, default 41943039): 41943039
Partition 2 of type Linux and of size 6 MiB is set

Command (m for help): wq
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.

```


