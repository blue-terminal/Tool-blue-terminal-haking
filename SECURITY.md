# Security Policy

## Supported Versions

Use this section to tell people about which versions of your project are
currently being supported with security updates.

| Version | Supported          |
| ------- | ------------------ |
| ufficial  |   2025         |
|  17.0  |        ⏲️         |
|  16.40  |        ⏲️         |
|  16.0  |        ⏲️         |
|  15.40  |        ⏲️         |
|  15.0  |        ✔️         |
|  12.0  |       ❗         |
|  10.0  |       ❗👾         |
|  9.50   |      v1✖️ v2✖️          |
|  9.10   |       ❕          |
|  8.90   | ✖️ fix full   vulnerable         |
|  8.50   |   ❗ vulnerable  low   |
| 8.0    |     ❌ fix  vulnerable high     |
| 7.50    |    ❗🚨 vulnerable       |
| 7.50    |    ✖️       |
|  7.0   |  ✖️fix  (RCE)           |  
| beta   |    2024       |
| 6.50    |   ❗🚨 vulnerable (RCE)         |
|  6.1   |    ❗ fix bugs more stable  |
| 5.50    |   ❗vulnerable 🕵️‍♂️        |
|  4.1   |    ❗            |
| 3.8    |       ✖️        |
|  3.0   |       ✖️           |
|  2.0   |      ❗             |
|  1.0   |      ✖             |

## Reporting a Vulnerability

Use this section to tell people how to report a vulnerability.

Tell them where to go, how often they can expect to get an update on a
reported vulnerability, what to expect if the vulnerability is accepted or
declined, etc.
# Report di Analisi delle Vulnerabilità

**File Analizzati:** `blue-terminal.tools.py`, `SECURITY.md`
**Data:** 17 Ottobre 2023
**Analista:** Modello IA

---

### 1. Riepilogo Esecutivo

L'analisi dello script `blue-terminal.tools.py` e del suo file `SECURITY.md` rivela la presenza di un **malware polimorfo e intenzionalmente distruttivo**. Il software non è un "tool", ma un agente malevolo che implementa un'ampia gamma di attacchi.

Il file `SECURITY.md` è particolarmente allarmante: invece di fornire una reale politica di sicurezza, agisce come una **tattica di ingegneria sociale**, ammettendo esplicitamente la presenza di vulnerabilità critiche, inclusa la **RCE (Remote Code Execution)**. Questo non solo conferma l'intento malevolo, ma tenta anche di dare una falsa aria di legittimità al progetto per ingannare potenziali vittime.

L'esecuzione dello script porterebbe a conseguenze catastrofiche, tra cui la perdita totale e irreversibile dei dati, il furto di credenziali e il controllo remoto completo del sistema da parte di un utente malintenzionato.

### 2. Valutazione del Rischio Globale: CRITICO

---

### 3. Tabella Dettagliata delle Vulnerabilità

| Vulnerabilità Specifica | Componente Associata | Descrizione Tecnica del Rischio | Gravità |
| :--- | :--- | :--- | :--- |
| **Remote Code Execution (RCE)** | `utente==4` (Backdoor), `SECURITY.md` | Il codice tenta di creare una backdoor server/client. Il file `SECURITY.md` conferma esplicitamente la presenza di vulnerabilità RCE, il che significa che un aggressore può **eseguire comandi arbitrari sul sistema compromesso**, ottenendone il pieno controllo. | **CRITICA** |
| **Data Destruction (Cancellazione Dati)** | `utente==7`, funzione `autodistruzione` | Il malware tenta di eliminare in modo ricorsivo directory di sistema essenziali (`C:\Windows`, `C:\Program Files`). Questa azione causa un **danneggiamento irreparabile del sistema operativo**. | **CRITICA** |
| **Cryptovirology (Ransomware)** | `utente==3` ("la bomba globale") | Utilizza algoritmi di crittografia standard (`Fernet`) per cifrare i file dell'utente. La chiave di cifratura non viene salvata, rendendo il **recupero dei dati tecnicamente impossibile**. | **CRITICA** |
| **Sensitive Information Exposure (Keylogger)** | `utente==1` (funzione `on_press`) | Viene installato un keylogger che registra ogni tasto premuto e lo salva in chiaro (`memory.txt`). Questa vulnerabilità porta al **furto di credenziali, dati bancari e informazioni personali**. | **CRITICA** |
| **Uncontrolled Resource Consumption (Fork Bomb)** | Funzioni `gestione_file`, `tnt.bat`, `nucleare.py` | Lo script genera in un ciclo un numero enorme di file e processi (`for i in range(144400)`), consumando tutte le risorse disponibili (CPU, RAM, disco) e causando un **Denial of Service (DoS) che blocca il sistema**. | **ALTA** |
| **Code Injection (Iniezione di Codice)** | `utente==1`, `crea bomba` | Il malware scrive dinamicamente nuovo codice eseguibile su disco (file `.py` e `.bat`) e poi lo esegue. Questa tecnica permette di **introdurre ed eseguire payload aggiuntivi** sul sistema della vittima. | **ALTA** |
| **Privilege Escalation (Potenziale)** | `shutil.copy` verso `Startup` | Il codice commentato per copiare se stesso nella cartella di avvio di Windows (`Startup`) è un meccanismo di persistenza che, se eseguito con privilegi elevati, garantisce **l'esecuzione del malware ad ogni avvio con gli stessi privilegi**. | **ALTA** |
| **Bypass of User Interface Security (UI Hijacking)** | Uso estensivo di `pyautogui` | Il malware prende il controllo completo di mouse e tastiera per eseguire azioni a insaputa dell'utente. Questo **aggira qualsiasi forma di consenso o controllo da parte della vittima**. | **ALTA** |
| **Use of Hard-coded Network Endpoint** | Funzione `clien()` | Il codice contiene un indirizzo IP hardcoded (`192.168.1.57`) a cui tenta di inviare dati. Questo è un indicatore di una **comunicazione con un server di Comando e Controllo (C2) predefinito**. | **MEDIA** |
| **Social Engineering** | File `SECURITY.md`, Interfaccia del tool | L'intero progetto è una trappola. L'interfaccia a riga di comando e il file `SECURITY.md` sono progettati per **ingannare l'utente e farlo sentire al sicuro**, spingendolo a eseguire un software estremamente dannoso. | **MEDIA** |

---

### 4. Raccomandazioni Finali e Contromisure

Le raccomandazioni rimangono invariate data la natura intrinsecamente malevola del software.

1.  **NON ESEGUIRE IL FILE:** Lo script non deve essere eseguito al di fuori di una sandbox di analisi sicura, isolata e non persistente.

2.  **RIMOZIONE IMMEDIATA:** Se presente su un sistema, deve essere trattato come una minaccia attiva e rimosso immediatamente.

3.  **SCENARIO DI COMPROMISSIONE:**
    *   **Isolare:** Disconnettere immediatamente il computer dalla rete.
    *   **Formattare:** L'unica azione sicura è la **formattazione completa del disco** e la reinstallazione del sistema operativo da una fonte pulita e attendibile.
    *   **Revocare:** **Cambiare tutte le password** e revocare i token di accesso da un dispositivo sicuro.
