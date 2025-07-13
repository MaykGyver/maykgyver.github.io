---
layout: post
title: Minimize Ubuntu 24.04 LTS on Strato VPS
date: "2025-07-13"
---

## Motivation

My internet carrier introduced carrier grade nat (cgnat). So I do not have a public ip address anymore. Goodbye dyndns!

There are several reports how one may workaround the missing public address with the help of a rented server. You connect your homelab to the server with the public address over a site-to-site vpn. Wireguard should be the weapon of choice these days. Be assured fellow reader that I will provide you my notes on this blog when I did accomplish all routings.

But hey, don't we need the server with the public address first? Yes buddy, we need to rent a server. I did some market research and fell in love with [Strato's VC1-1](https://www.strato.de/server/linux-vserver/). Priced at 1€ per month it should be ok for a box that sits there waiting during most of the time. If the desire grows you have the option to upgrade at any time.

I want basic connectivity. No low-latency or high-throughput is expected. But still, when doing the first `apt update; apt upgrade` the box feels ... well ... flabby. Even for the price.

## Does Strato install Ubuntu Server Standard or Ubuntu Server Minimized?

That made me curios. At Strato VC you are not allowed to insert an ISO file to install from. You have a menu with fifteen options.

* AlmaLinux 8
* AlmaLinux 8 + Plesk
* AlmaLinux 9
* AlmaLinux 9 + Plesk
* Debian 11
* Debian 11 + Plesk
* Debian 12
* Debian 12 + Plesk
* Rocky Linux 8
* Rocky Linux 8 + Plesk
* Rocky Linux 9
* Ubuntu 22.04
* Ubuntu 22.04 + Plesk
* Ubuntu 24.04
* Ubuntu 24.04 + Plesk

Plesk is ruled out quickly. Out of pure caprice I went for Ubuntu 24.04 LTS. But looking at the desastrous performance of the server even without applications installed I wonder: did they provide a standard or a minimized install of Ubuntu Server?

To figure out, I configured two VMs on my local HyperV and installed the two considered scenarios for comparison. Then I measured a bit using

```bash
apt list --installed | wc -l
apt list --manual-installed
df -h
```

Here are the results:


| | Ubuntu 24.04<br>Hyper-V<br>Server standard | Ubuntu 24.04<br>Hyper-V<br>Server mimimized | Ubuntu 24.04<br>Strato VPS<br>as delivered | candidate <br>for<br>removal | Ubuntu 24.04<br>Strato VPS<br>customized |
| ------------------------ | :---: | :---: | :---: | :---: | :---: |
| base-files               | ⬤ | ⬤ | ⬤ |   | ⬤ |
| bash                     | ⬤ | ⬤ | ⬤ |   | ⬤ |
| bsdutils                 | ⬤ | ⬤ | ⬤ |   | ⬤ |
| cloud-init               |   |   | ⬤ | ⬤ |   |
| dash                     | ⬤ | ⬤ | ⬤ |   | ⬤ |
| debianutils              |   | ⬤ |   |   |   |
| diffutils                | ⬤ | ⬤ | ⬤ |   | ⬤ |
| eatmydata                |   |   | ⬤ | ⬤ |   |
| efibootmgr               | ⬤ | ⬤ |   |   |   |
| findutils                | ⬤ | ⬤ | ⬤ |   | ⬤ |
| grep                     | ⬤ | ⬤ | ⬤ |   | ⬤ |
| grub-efi-amd64-signed    | ⬤ | ⬤ |   |   |   |
| grub-efi-amd64           | ⬤ | ⬤ |   |   |   |
| grub-pc                  |   |   | ⬤ |   | ⬤ |
| gzip                     | ⬤ | ⬤ | ⬤ |   | ⬤ |
| hostname                 | ⬤ | ⬤ | ⬤ |  | ⬤ |
| init                     | ⬤ |   | ⬤ | X | ⬤ |
| libc-bin                 |   | ⬤ |   |   |   |
| libeatmydata1            |   |   | ⬤ | ⬤ |   |
| libwrap0                 |   |   | ⬤ | ⬤ |   |
| linux-generic            | ⬤ | ⬤ |   |   |   |
| linux-virtual            |   |   | ⬤ |   | ⬤ |
| login                    | ⬤ | ⬤ | ⬤ |   | ⬤ |
| lxd-installer            |   | ⬤ |   |   |   |
| mawk                     |   | ⬤ |   |   |   |
| ncurses-base             | ⬤ | ⬤ | ⬤ |   | ⬤ |
| ncurses-bin              | ⬤ | ⬤ | ⬤ |   | ⬤ |
| ncurses-term             |   |   | ⬤ | ⬤ |   |
| openssh-server           | ⬤  | ⬤ | ⬤ |   | ⬤ |
| openssh-sftp-server      |   |   | ⬤ | ⬤ |   |
| python-babel-localedata  |   |   | ⬤ | ⬤ |   |
| python3-babel            |   |   | ⬤ | ⬤ |   |
| python3-jinja2           |   |   | ⬤ | ⬤ |   |
| python3-json-pointer     |   |   | ⬤ | ⬤ |   |
| python3-jsonpatch        |   |   | ⬤ | ⬤ |   |
| python3-jsonschema       |   |   | ⬤ | ⬤ |   |
| python3-markupsafe       |   |   | ⬤ | ⬤ |   |
| python3-pyrsistent       |   |   | ⬤ | ⬤ |   |
| python3-serial           |   |   | ⬤ | ⬤ |   |
| python3-tz               |   |   | ⬤ | ⬤ |   |
| shim-signed              | ⬤ | ⬤ | ⬤ |   | ⬤ |
| ssh-import-id            |   |   | ⬤ | ⬤ |   |
| ubuntu-minimal           | ⬤ | ⬤ | ⬤ |   | ⬤ |
| ubuntu-server            | ⬤ |   | ⬤ | ⬤ |   |
| ubuntu-standard          | ⬤ |   | ⬤ | ⬤ |   |
| util-linux               | ⬤ | ⬤ | ⬤ |   | ⬤ |
| ∑ marked packages        | 23 | 24 | 38 | 19 | 19 |
| ∑ all installed packages | 682 | 470 | 666 |   | 405 |
| reboot duration          | 8.78 s ±0.23 s | 7.98 s ±0.14 s | 23.75 s ±4.92 s | | 8.46 s ±0.29 s |

The following lines reduce your Strato VPS too within a couple moments.

```bash
apt purge ubuntu-standard
apt purge ubuntu-server
apt purge ssh-import-id
apt purge python3-tz
apt purge python3-serial
apt purge python3-pyrsistent
apt purge python3-markupsafe
apt purge python3-jsonschema
apt purge python3-jsonpatch
apt purge python3-json-pointer
apt purge python3-jinja2
apt purge python3-babel
apt purge python-babel-localedata
apt-mark auto openssh-sftp-server
apt purge ncurses-term
apt-mark auto libwrap0
apt purge libeatmydata1
# apt purge init
apt purge eatmydata
apt purge cloud-init
apt autoremove --purge
reboot now
```

I have used such a customized machine for some weeks now and found no downside of the miniaturization. Leave a message if you found any issues with this recipe.
