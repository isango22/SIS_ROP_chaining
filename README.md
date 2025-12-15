# 🛡️ Simulacija ROP Napada (ROP Emporium – `split`)

Ovaj projekt demonstrira izvođenje **Return-Oriented Programming (ROP)** napada u **izoliranom mrežnom laboratoriju** koristeći dvije virtualne mašine. Cilj je iskoristiti ranjivost u binarnoj datoteci **`split`** (iz ROP Emporium izazova) koja se izvršava kao **mrežni servis** na udaljenom poslužitelju.

> ⚠️ **Disclaimer**: Projekt je izrađen isključivo u **edukativne svrhe**, u kontroliranom kućnom laboratoriju. Ne koristiti ove tehnike na sustavima za koje nemate izričito dopuštenje.

---

## 🏗️ Arhitektura Laboratorija

Laboratorij se sastoji od dvije virtualne mašine povezane putem **VirtualBox NAT Network** mreže:

### 🧑‍💻 Napadač (Attacker)

* **OS:** Kali Linux
* **Alati:**

  * `pwntools`
  * `Cutter` (GUI za Rizin)
  * `ROPgadget`
  * `netcat`

### 🖥️ Meta (Victim)

* **OS:** Debian 9 (x64)
* **Napomena:**

  * Starija verzija sustava
  * **ASLR ručno isključen** radi edukativne vježbe
  * Ranjiva aplikacija pokrenuta kao mrežni servis

---

## 🛠️ Korištene Tehnike

Projekt simulira puni **Attack Kill Chain** proces:

### 🔍 Post-Exploitation Izviđanje

* Pristup meti putem SSH-a
* Tiha identifikacija zanimljivih datoteka:

  ```bash
  ls -lat
  find /
  ```

### 🕵️ Stealth Eksfiltracija

* Krađa binarne datoteke s mete na napadača
* Korištenje `netcat` umjesto SCP/FTP-a radi smanjenja traga u logovima

### 🧪 Statička Analiza

* Analiza binarne datoteke pomoću **Cuttera**
* Identifikacija kritičnih funkcija:

  * `pwnme`
  * `system`

### 🧩 Prikupljanje ROP Gadgeta

* Pronalaženje potrebnih gadgeta pomoću `ROPgadget`:

  * `pop rdi ; ret`
* Identifikacija adresa stringova:

  * `/bin/cat flag.txt`

### 🌐 Remote Exploitation

* Izrada **Python exploit skripte** koristeći `pwntools`
* Slanje malicioznog payload-a kroz mrežu
* Preuzimanje kontrole nad **RIP registrom**
* Izvršavanje naredbe na udaljenom sustavu

---

## 🚀 Kako Pokrenuti

### 1️⃣ Priprema Mete (Debian)

Isključivanje ASLR-a i pokretanje ranjive aplikacije kao mrežnog servisa na portu **1337**:

```bash
# Isključivanje ASLR-a (kao root)
echo 0 > /proc/sys/kernel/randomize_va_space

# Pokretanje servisa
socat TCP-LISTEN:1337,fork,reuseaddr EXEC:./split
```

---

### 2️⃣ Pokretanje Napada (Kali)

Pokretanje exploit skripte koja se spaja na metu i dohvaća zastavicu:

```bash
python3 exploit.py
```

---

## 📂 Struktura Repozitorija

```text
.
├── exploit.py      # Python skripta za izvođenje ROP napada (pwntools)
├── split           # Ranjiva binarna datoteka (ROP Emporium izazov)
├── writeup.md      # Detaljan opis analize i koraka (opcionalno)
└── README.md       # Ovaj dokument
```

---

## 🎯 Cilj Projekta

* Razumijevanje **ROP napada** u realističnom mrežnom scenariju
* Rad s binarnom analizom i exploit razvojem
* Simulacija napadačkog lanca od izviđanja do eksploatacije
* Sigurno učenje u izoliranom laboratorijskom okruženju

---

## 📚 Reference

* [ROP Emporium](https://ropemporium.com/)
* `pwntools` dokumentacija
* `ROPgadget` alat

---

