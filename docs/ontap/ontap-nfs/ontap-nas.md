# Playbook-översikt: NFS-provisionering i ONTAP

Denna playbook automatiserar hela det standardiserade 3-stegsflödet för att driftsätta en tillgänglig NFS-export inom en Storage VM (SVM) via ONTAP REST API[cite: 2, 3].

---

## 1. Huvudstruktur & Funktioner

* **Körning på `localhost`:** Modulerna körs som lokala Python-anrop från kontrollnoden och kommunicerar över HTTPS (port 443) mot klustrets Cluster Management LIF[cite: 2, 3].
* **`module_defaults`:** Eliminerar upprepning av parametrar (`hostname`, `username`, `password`, `vserver`, certifikat) så att varje enskild task förblir ren och koncis[cite: 3].
* **`vars_prompt`:** Läser in administratörslösenordet dolt vid körning så att inga känsliga inloggningsuppgifter sparas i klartext i koden[cite: 3].

---

## 2. Arbetsflöde i Tasks (3 steg)

1. **Skapa Export Policy (`na_ontap_export_policy`):**
   * Skapar en tom behållare för exportregler under angiven SVM[cite: 2, 3].
   * Standardstatus i ONTAP: Tom policy = all nätverksaccess nekas[cite: 2].

2. **Skapa Export Policy-regel (`na_ontap_export_policy_rule`):**
   * Tilldelar åtkomst för definierat klientsubnät (`client_match`)[cite: 2].
   * Sätter protokoll och rättigheter (`ro_rule: sys`, `rw_rule: sys`)[cite: 2].
   * Sätter `super_user_security: sys` för att undvika *root squashing* (så att UID 0 inte mappas om till `nobody`/`65534`)[cite: 2].

3. **Skapa FlexVol (`na_ontap_volume`):**
   * Tilldelar volymen till det fysiska aggregatet (`aggregate_name`)[cite: 2, 3].
   * Applicerar den nyskapade policyn via `policy`[cite: 2].
   * Monterar volymen i SVM:ens katalogträd via `junction_path: "/{{ volname }}"`[cite: 2, 3].
   * Konfigurerar `space_guarantee: "none"` (Thin Provisioning) för optimal lagringseffektivitet på flash/SSD[cite: 1, 3].

---

## 3. Parametrar & Variabler

| Variabel | Beskrivning | Exempelvärde |
| :--- | :--- | :--- |
| `netapp_hostname` | Klustrets Management LIF | `172.0.10.100` (A220) / `192.168.0.101` (Labb)[cite: 1, 2] |
| `netapp_username` | Klusteradministratör | `admin`[cite: 1, 2] |
| `vserver` | Mål-SVM (Tenant) | `svm_proxmox` (A220) / `svm1_cluster1`[cite: 1, 2] |
| `volname` | Basnamn för volym och policy | `vol1` / `vm_data_01`[cite: 2] |
| `client_match` | Beviljat klientnätverk | `10.1.31.0/24` (A220 VLAN 301) / `192.168.0.0/24`[cite: 1, 2] |
| `aggr_name` | Fysiskt aggregat | `aggr1_01` (A220) / `cluster1_01_SSD_1`[cite: 1, 2] |
| `size` | Volymens kapacitet (GB) | `10`[cite: 2] |

---

## 4. Verifiering efter körning (Clustershell CLI)

```text
# Kontrollera volymens status, storlek, garanti och junction path
volume show -vserver <vserver> -volume <volname> -fields state,size,space-guarantee,policy,junction-path[cite: 2]

# Verifiera att monteringen i SVM-namnrymden är aktiv
volume show-mounting -vserver <vserver> -volume <volname>[cite: 2]

# Verifiera exportregeln
vserver export-policy rule show -vserver <vserver> -policyname <volname>_policy[cite: 2]