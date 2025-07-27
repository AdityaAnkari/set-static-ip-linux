# set-static-ip-linux
Step-by-step guide for configuring a static IP address in Linux (Ubuntu, CentOS, Debian, etc.)
# 🌐 Set Static IP Address in Linux

This repository is a beginner-friendly, step-by-step guide to configuring a **static IP address** on Linux systems.

Whether you're setting up a bioinformatics server, a local machine, or a Raspberry Pi — assigning a static IP ensures your machine has a **permanent network address** (so it doesn’t change on reboot or reconnect).

---

## 📘 Why Use a Static IP?

| Situation | Why Static IP Helps |
|-----------|----------------------|
| Running remote jobs via SSH | Your IP won’t change every reboot |
| Using NFS, Samba, or web servers | Clients can reliably find your system |
| Building a local cluster | Each node needs a fixed address |
| Port forwarding or firewall rules | Rules depend on stable IPs |

---

## 🖥️ How to Set Static IP on Different Systems

### 🐧 Ubuntu (20.04+ / netplan)

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Example configuration:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Then apply changes:

```bash
sudo netplan apply
```

---

### 🐧 Debian / Ubuntu Older (< 18.04)

```bash
sudo nano /etc/network/interfaces
```

Example:

```bash
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 1.1.1.1
```

Restart networking:

```bash
sudo systemctl restart networking
```

---

### 🐧 CentOS / RHEL

Edit your interface file (use `ip a` to find the name):

```bash
sudo nano /etc/sysconfig/network-scripts/ifcfg-eth0
```

Example:

```ini
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

Then restart the network:

```bash
sudo systemctl restart network
```

---

### 🧠 Find Your Interface Name

```bash
ip link
# OR
ip a
```

Look for names like `eth0`, `ens33`, or `enp0s3`.

---

## 📡 Tips and Troubleshooting

| Problem | Fix |
|--------|-----|
| No internet after setting IP | Check gateway and DNS |
| `netplan apply` fails | Check YAML syntax (indentation!) |
| Can't connect to server | Confirm IP with `ip a` |
| Still using DHCP | Ensure `dhcp4: no` or `BOOTPROTO=static` |

---

## 📌 TL;DR Summary

| Distro | Config File |
|--------|-------------|
| Ubuntu 20.04+ | `/etc/netplan/*.yaml` |
| Ubuntu < 18.04 / Debian | `/etc/network/interfaces` |
| CentOS / RHEL | `/etc/sysconfig/network-scripts/ifcfg-*` |

---

## ✅ Recommended Next Steps

- Test with `ping 8.8.8.8` and `ping google.com`
- Set up SSH access
- Use static IP in your `/etc/hosts` file for easy naming
- Learn `nmcli` (NetworkManager CLI) for desktop Linux systems

---

## 📁 Want More?

You can contribute:
- Configs for Raspberry Pi (`dhcpcd.conf`)
- Configs using `nmcli` or `nmtui`
- Add scripts to auto-switch static/dynamic modes

Pull requests welcome!

