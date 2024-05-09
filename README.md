# RISC-V emulator docker image

A pre-configured QEMU & Debian RISC-V image. Allows you to get started working on an emulated RISC-V environment. Is a quick way to see what libraries and frameworks work on RISC-V without sourcing hardware.

Structure:

- Debian host (debian:sid)
  - QEMU. Running as emulated RISC-V with soft-mmu support
    - Debian guest. RISC-V image
    - RISC-V u-boot kernel

## How to use

### Quicker start, with Docker Hub:

Pull and run the image from Docker Hub. Saves you needing to build locally 
 - <https://hub.docker.com/r/davidburela/riscv-emulator>

### Quick start:

#### Get the the image

```bash
# 1 Get the image [2 options]
# 1A. Pull the image from Docker Hub
docker pull davidburela/riscv-emulator

# 1B. -OR- build the image locally
docker build -t davidburela/riscv-emulator .

# 2. 
# Run with QEMU default of 2CPU & 2G ram. 
# Expose port 2222 which is routed through into the QEMU RISC-V guest
docker run -d --publish 127.0.0.1:2222:2222/tcp davidburela/riscv-emulator

# 3. SSH directly into the QEMU RISC-V guest, the default password is "root". (Might take a few minutes for guest to start)
ssh root@localhost -p 2222
```

![SSH in and seeing CPU details](ssh-riscv-cpu.png)

### Options (RAM, CPU, etc)

```bash
# 2. Alternative: Allocate more CPU / RAM to QEMU guest
# Amount of memory and CPU count are available as docker variables. 
# QM_CPU=X where X is the number of CPUs to be added. 
# QM_RAM=Y where Y is the amount of memory, i.e. 4G means 4 gigabytes of RAM.
docker run -d --publish 127.0.0.1:2222:2222/tcp -e QM_CPU=4 -e QM_RAM=4G davidburela/riscv-emulator
```

### Podman

Declare docker.io registry declared in /etc/containers/registries.conf, i.e.

```conf
[registries.search]
registries = [ 'docker.io', 'quay.io' ]
```

Build with

```bash
podman build -t riscv-emulator
```

For other docker sub-commands, just replace `docker` with `podman`

## Example uses

I have used the emulated environment to verify if Open Source projects run on RISC-V, and shared the findings:
- [Golang](https://blog.davidburela.com/2020/11/21/cross-compiling-golang-for-risc-v/)
- [IPFS](https://blog.davidburela.com/2020/11/16/ipfs-on-risc-v/)
- [Ethereum](https://blog.davidburela.com/2020/12/03/ethereum-on-risc-v/)

## How it was built

- Was built based on the steps in my blog post <https://blog.davidburela.com/2020/11/15/emulating-risc-v-debian-on-wsl2/>
- Utilising [Giovanni Mascellani’s prebaked Debian images](https://www.giovannimascellani.eu/dqib-debian-quick-image-baker.html)
