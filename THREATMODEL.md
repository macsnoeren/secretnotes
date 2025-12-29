# Threat Model – Local Browser-Based Password Manager (password-manager.html)

## 1. Scope & Doel

Deze applicatie is een **client-side password manager** die:

- Volledig draait in de browser
- Geen server- of netwerkcommunicatie gebruikt
- Gevoelige data versleuteld opslaat in `localStorage`
- Beveiliging baseert op een master password

**Doel van dit threat model:**
- Identificeren van bedreigingen
- Vaststellen welke risico’s worden gemitigeerd
- Expliciet maken welke risico’s **niet** worden opgelost

---

## 2. Assets (Wat beschermen we?)

| Asset | Beschrijving |
|-------|-------------|
| Vault inhoud | Wachtwoorden, usernames, notities |
| Master password | Sleutel tot alle data |
| Afgeleide cryptografische sleutels | PBKDF2 output |
| Clipboard data | Tijdelijk gekopieerde wachtwoorden |
| In-memory plaintext | Data tijdens gebruik |
| Vault integriteit | Bescherming tegen manipulatie |

---

## 3. Trust Boundaries

| Boundary | Vertrouwen |
|----------|------------|
| Browser runtime | Vertrouwd |
| SJCL library | Vertrouwd (audited crypto) |
| localStorage | Onbetrouwbaar (kan worden aangepast) |
| Besturingssysteem | Semi-vertrouwd |
| Andere browser tabs/extensions | Onbetrouwbaar |
| Gebruiker | Kan fouten maken |

---

## 4. Aanvalsoppervlak

- localStorage (lezen/schrijven)
- Clipboard
- DOM & JavaScript runtime
- Browser extensions
- Physical access tot apparaat
- Browser debugging tools

---

## 5. Threats & Mitigations (STRIDE)

### 5.1 Spoofing (Identiteitsmisbruik)

**Threat:** Aanvaller probeert vault te openen zonder correct master password.  
**Mitigatie:**  
- PBKDF2-HMAC-SHA256  
- AES-CCM (authenticated encryption)  
- Foute password → decrypt faalt  

**Status:** ✅ Gemitigeerd

---

### 5.2 Tampering (Data manipulatie)

**Threat:** Aanvaller wijzigt encrypted vault in localStorage.  
**Mitigatie:**  
- AES-CCM authenticatie tag  
- `ccm: tag doesn't match` bij wijziging  
- Vault wordt niet geladen  

**Status:** ✅ Gemitigeerd

---

### 5.3 Repudiation (Ontkenning)

**Threat:** Gebruiker kan niet bewijzen wie data heeft gewijzigd.  
**Mitigatie:** Niet van toepassing (single-user, lokaal)  

**Status:** ⚠️ Out of scope

---

### 5.4 Information Disclosure (Data uitlekken)

| Scenario | Mitigatie | Status |
|----------|-----------|--------|
| localStorage uitlezen | Sterke encryptie | ✅ |
| Clipboard sniffing | Auto-wipe | ⚠️ Best effort |
| DOM inspectie | Auto-lock + hidden fields | ⚠️ Beperkt |
| Memory dump | Best-effort wipe | ⚠️ Niet perfect |

**Belangrijk:** Browser-geheugen kan **niet volledig veilig gewist** worden door JavaScript.

---

### 5.5 Denial of Service

**Threat:** Vault wordt onbruikbaar gemaakt (corruptie).  
**Mitigatie:** Detectie via CCM tag → fail-secure  

**Status:** ✅ Gedetecteerd (niet te voorkomen)

---

### 5.6 Elevation of Privilege

**Threat:** Kwaadaardige JS krijgt toegang tot vault.  
**Mitigatie:**  
- Geen eval  
- Geen externe input  
- Geen server  
- Minimal attack surface  

**Niet gemitigeerd:**  
- Kwaadaardige browser extensions  
- XSS via user-injected HTML (niet aanwezig)  

**Status:** ⚠️ Gedeeltelijk

---

## 6. Cryptografisch Ontwerp

| Component | Keuze |
|-----------|-------|
| Encryptie | AES-256-CCM |
| KDF | PBKDF2-HMAC-SHA256 |
| Iteraties | 250.000 |
| Authenticatie | Ingebouwd (CCM) |
| Salt | Random, per vault |
| Library | SJCL |

**Eigenschappen:**  
- AEAD (encrypt + integrity)  
- Misuse resistant  
- Geen custom crypto

---

## 7. Out-of-Scope Threats (Expliciet!)

Deze applicatie beschermt **NIET** tegen:

- Kwaadaardige browser extensions
- Gecompromitteerd OS
- Keyloggers
- Screen capture malware
- Browser exploits
- Cold-boot attacks
- Forensische memory dumps

➡️ Dit is **acceptabel** voor een browser-only manager.

---

## 8. Secure Defaults & UX-Mitigations

| Feature | Doel |
|---------|------|
| Auto-lock | Minimaliseert exposure |
| Clipboard auto-wipe | Beperkt lekken |
| Password hidden by default | Shoulder surfing |
| Confirm dialogs | Human error |
| Integrity fail → hard stop | Fail-secure |

---

## 9. Residual Risk

| Risico | Acceptatie |
|--------|------------|
| Clipboard leakage | Acceptabel |
| Browser memory persistence | Acceptabel |
| User kiest zwak password | Gedocumenteerd |
| localStorage loss | Acceptabel |

---

## 10. Security Posture Samenvatting

**Deze applicatie is:**

✔️ Zero-knowledge  
✔️ Offline-only  
✔️ Cryptografisch correct  
✔️ Tamper-detecting  
✔️ Fail-secure  
✔️ Geen server trust  

**Maar:**

❗ Geen vervanging voor native password managers  
❗ Browser sandbox limits blijven gelden  

---

## 11. Aanbevolen Volgende Hardening (optioneel)

1. 🔐 Argon2 via WASM  
2. 🔒 Per-item encryption  
3. ⏱️ Password reveal timeout  
4. 🧪 Vault integrity self-test  
5. 🔍 CSP headers (indien gehost)  

---

## 12. Conclusie

> Binnen het gekozen domein (browser-only, offline, zero-server)  
> is dit ontwerp **cryptografisch solide** en **professioneel verantwoord**.
