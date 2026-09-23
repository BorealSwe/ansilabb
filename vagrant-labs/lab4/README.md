# Ansible Lab: Multi-OS Cluster (Rocky Linux 9 & Ubuntu 22.04)

Denna katalog innehåller en modulär `Vagrantfile` som sätter upp en labbmiljö med tre noder avsedda för Ansible-automatisering. Konfigurationen är provider-agnostisk och stödjer både **KVM/libvirt** och **VirtualBox**.

---

## Topologi och Nätverk

Alla noder tilldelas statiska IP-adresser på ett isolerat privat nätverk (`192.168.57.0/24`).

| Nod | Hostname | IP-adress | vCPU | RAM | Basavbild (Box) | Distribution |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Node 1 | `rocky1` | `192.168.57.10` | 1 | 1024 MB | `generic/rocky9` | Rocky Linux 9 (RHEL) |
| Node 2 | `rocky2` | `192.168.57.11` | 1 | 1024 MB | `generic/rocky9` | Rocky Linux 9 (RHEL) |
| Node 3 | `ubuntu1` | `192.168.57.12` | 1 | 1024 MB | `generic-x64/ubuntu2204` | Ubuntu 22.04 LTS (Debian) |

---

## Förutsättningar

* **Vagrant** (>= 2.4.x)
* Någon av följande hypervisors konfigurerad på värdmaskinen:
  * **KVM/libvirt** med pluginet `vagrant-libvirt`
  * **Oracle VirtualBox**

---

## Snabbstart

### 1. Starta noderna

Kör kommandot baserat på vilken provider du vill använda:

* **För KVM / libvirt:**
  ```bash
  vagrant up --provider=libvirt
