```shell
我把/dev/sda8 挂在到了/mnt
卸载的时候发现设备忙
用fuser查看是什么在使用导致无法正常卸载

#umount /dev/sda8
umount: /mnt: target is busy
(In some cases useful info about processes that use the device is found by lsof(8) or fuser(1).)

#fuser -u -a -i -m -v /mnt
USER        PID ACCESS COMMAND
/mnt:                root     kernel mount (root)/mnt
		     root       5618 ..c.. (root)adb
发现是adb导致的
查看adb程序确实在运行
ps aux | grep adb
root      5618  0.0  0.0 170360  3168 ?        Sl   Jan30   0:39 adb -P 5037 fork-server server
root     21244  0.0  0.0  15824  2504 pts/3    S+   14:40   0:00 grep --colour=auto adb

停止ADB程序之后就可以正常卸载了
#adb kill-server
```