# Ansible Lab: Rocky Linux 9 Cluster

Detta projekt tillhandahåller en automatiserad labbmiljö med tre virtuella Rocky Linux 9-noder avsedda för Ansible-automatisering och tester.

Miljön är anpassad för att fungera sömlöst på både **KVM/libvirt** och **VirtualBox**.

---

## Arkitektur

| Nod | Hostname | IP-adress | vCPU | RAM | OS-avbild |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Node 1 | `rocky1` | `192.168.57.10` | 1 | 1024 MB | generic/rocky9 |
| Node 2 | `rocky2` | `192.168.57.11` | 1 | 1024 MB | generic/rocky9 |
| Node 3 | `rocky3` | `192.168.57.12` | 1 | 1024 MB | generic/rocky9 |

Noderna binds samman via ett internt host-only/privat nätverk (`192.168.57.0/24`).

---

## Förutsättningar

* [Vagrant](https://developer.hashicorp.com/vagrant/downloads) (>= 2.4.x)
* Någon av följande hypervisors installerad:
  * **KVM/libvirt** (med pluginet `vagrant-libvirt`)
  * **Oracle VirtualBox**

---

## Användning

### 1. Starta klustret

Starta med önskad provider:

* **KVM / libvirt:**
  ```bash
  vagrant up --provider=libvirt
