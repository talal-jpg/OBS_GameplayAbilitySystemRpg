## 1.Check where Virt-Manager is trying to save the disk

When creating the VM:

1. Check **Customize before install**.
2. Before clicking Finish, look at **Storage**.
3. It will probably say something like

```
/var/lib/libvirt/images/ubuntu.qcow2
```

or maybe

```
/media/talal/vm/ubuntu.qcow2
```

If it's pointing to the FAT partition, that's the problem.

## 2.Another common issue

If you mounted the drive through Nemo, it is often owned by your user:

```
talal
```

but libvirt runs as

```
libvirt-qemu
```

or

```
qemu
```

which **cannot write there**.

Check permissions:

```
ls -ld /media/talal/vm
```

Also check:

```
namei -l /media/talal/vm
```



callMyJoin(Exec) custom func wrapper in cont or char class that calls subsystem func 