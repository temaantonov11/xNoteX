>[!info] Architecture
> RISC-V

## OS

[AlpineU-Boot](https://dl-cdn.alpinelinux.org/alpine/v3.21/releases/riscv64/alpine-uboot-3.21.2-riscv64.tar.gz)

## Packages on WSL

```shell
sudo apt install qemu-system
sudo apt install virt-manager
wget <link to .ISO>
```

## Export environment variable

```shell
export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0
```
## Run Virtual Machine

>[!info] VM Startup algorithm
>**1 step**. Run XLaunch
>**2 step**. Run virt-manager (`sudo virt-manager`)


## Features

- **QEMU API**
- **How run virt-manager without `sudo`**
- **QCOW2 vs RAW**

## Notes

- **Create custom storage** 
- **Disable allocate entire volume row**






