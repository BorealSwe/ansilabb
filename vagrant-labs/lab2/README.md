# Ansible & Vagrant Lab Environment

Lokal testmiljö byggd med Vagrant och VirtualBox för att öva Ansible-automation mot Rocky Linux 9-noder.

## Nätverk & Noder

Bas-box: `generic/rocky9` (1 vCPU, 1024 MB RAM, Linked Clones aktiverat)

| Nod | Roll | IP-adress | Hostname |
| :--- | :--- | :--- | :--- |
| `control` | Ansible Control Node | `192.168.56.10` | `orc-control.test` |
| `app1` | Managed Node (App 1) | `192.168.56.7` | `orc-app1.test` |
| `app2` | Managed Node (App 2) | `192.168.56.8` | `orc-app2.test` |
| `db` | Managed Node (Database) | `192.168.56.9` | `orc-db.test` |

---

## Förutsättningar

* [Vagrant](https://www.vagrantup.com/) (v2.x+)
* [Oracle VirtualBox](https://www.virtualbox.org/)

---

## Snabbstart

1. **Starta alla virtuella maskiner:**
   ```bash
   vagrant up
