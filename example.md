# Proxmox
# Proxmox VE – LXC konteinerio sukūrimas

## Tikslas

Sukurti LXC konteinerį Proxmox VE aplinkoje naudojant GUI arba terminalo komandas.

---

## 1. LXC šablono atsisiuntimas

### GUI būdu

1. Prisijungti prie Proxmox VE Web GUI.
2. Pasirinkti mazgą (Node).
3. Atidaryti **Storage → local (pve) → CT Templates**.
4. Paspausti **Templates**.
5. Pasirinkti norimą šabloną (pvz. `debian-12-standard`) ir atsisiųsti.

### Terminale

```bash
pveam update
pveam available
pveam download local debian-12-standard_12.7-1_amd64.tar.zst
```

---

## 2. LXC konteinerio sukūrimas naudojant GUI

1. Paspausti **Create CT**.
2. Nurodyti:

   * CT ID: `100`
   * Hostname: `debian-lxc`
   * Password: pasirinktas slaptažodis
3. Pasirinkti atsisiųstą šabloną.
4. Disk:

   * Storage: `local-lvm`
   * Disk size: `8 GB`
5. CPU:

   * Cores: `2`
6. Memory:

   * RAM: `1024 MB`
7. Network:

   * Bridge: `vmbr0`
   * DHCP arba statinis IP
8. Patvirtinti ir spausti **Finish**.

---

## 3. LXC konteinerio sukūrimas terminale

```bash
pct create 100 \
local:vztmpl/debian-12-standard_12.7-1_amd64.tar.zst \
--hostname debian-lxc \
--cores 2 \
--memory 1024 \
--swap 512 \
--rootfs local-lvm:8 \
--net0 name=eth0,bridge=vmbr0,ip=dhcp \
--password Slaptazodis123
```

---

## 4. Konteinerio paleidimas

```bash
pct start 100
```

Patikrinti būseną:

```bash
pct status 100
```

---

## 5. Prisijungimas prie konteinerio

```bash
pct enter 100
```

Patikrinti OS versiją:

```bash
cat /etc/os-release
```

---

## 6. Konteinerio stabdymas

```bash
pct stop 100
```

---

## 7. Konteinerio perkrovimas

```bash
pct reboot 100
```

---

## 8. Konteinerių sąrašas

```bash
pct list
```

---

## Rezultatas

Sukurtas ir paleistas LXC konteineris:

* CT ID: 100
* Hostname: debian-lxc
* OS: Debian 12
* CPU: 2 branduoliai
* RAM: 1024 MB
* Diskas: 8 GB
* Tinklas: vmbr0 (DHCP)
