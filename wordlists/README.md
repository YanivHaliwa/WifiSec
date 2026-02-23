# Wordlists

> ### Well, I can't include the full wordlists here — they're massive. But below are ideas and commands to generate the most effective ones for WiFi testing.

---

## Included — wifi.txt

A compact curated list of ~4,800 common WiFi passwords. Fast to run, good first attempt before pulling out the heavy lists.

```bash
crackcap capture.cap -l wordlists/wifi.txt
```

---

## 1 — 8-Digit Numbers

Pure numeric passwords with leading zeros. Covers all 8-digit combinations (100M entries).

```bash
seq -w 0 99999999 > 8_digit.txt
```

---

## 2 — Phone Number Formats

People commonly use their phone number as a WiFi password.

**Israel (05X — 10 digits):**

```bash
python3 -c "[print(f'05{i:08d}') for i in range(10**8)]" > il_mobile.txt
```

**Philippines (09XX — 11 digits):**

```bash
python3 -c "[print(f'09{i:09d}') for i in range(40000000, 940000000)]" > ph_mobile.txt
```

---

## 3 — 2 Letters + 6 Digits

A very common WiFi password pattern — two letters followed by six digits (e.g. `ab123456`).

```bash
python3 -c "
import itertools, string
chars = string.ascii_lowercase + string.ascii_uppercase
[print(a+b+f'{n:06d}') for a,b in itertools.product(chars, repeat=2) for n in range(10**6)]
" > 2char6digit.txt
```

---

## 4 — Router-Specific Format (e.g. PLDT)

Some routers ship with predictable default passwords based on their SSID prefix. Generate a targeted list for that pattern.

**PLDT (format: `PLDTWIFIxxxxx`):**

```bash
python3 -c "
import itertools, string
chars = string.ascii_lowercase + string.digits
[print('PLDTWIFI'+''.join(c)) for c in itertools.product(chars, repeat=5)]
" > pldt-wifi.txt
```

Adapt the prefix and length to match whatever router brand you're targeting.

---

## 5 — RockYou

Always worth trying. Contains real leaked passwords from millions of users.

```bash
# Usually already on Kali:
/usr/share/wordlists/rockyou.txt

# Or download:
gzip -d /usr/share/wordlists/rockyou.txt.gz
```

---

## Legal Notice

Use these wordlists only on networks you own or have explicit written permission to test. Unauthorized access is illegal.

Created by [Yaniv Haliwa](https://github.com/YanivHaliwa) for security testing.
