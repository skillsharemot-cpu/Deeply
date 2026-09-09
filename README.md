# Jobbmint – Árkalkulátor és árajánlat-készítő

Ideiglenes munkafelület a Jobbmint (kárpittisztítás) belső eszközeihez.
Éles link: **https://skillsharemot-cpu.github.io/Deeply/**

Később az egész átkerül a végleges honlap szerverére – addig ez a legegyszerűbb
mód arra, hogy telefonon és más gépen is megnyíljanak az eszközök.

## Mi van benne?

| Fájl | Mire való | Kinek |
|---|---|---|
| `index.html` | Belépő oldal, innen nyílik a három eszköz | – |
| `arkalkulator_foglalas.html` | Vevői árkalkulátor + foglalási varázsló | vevőknek küldhető |
| `arkalkulator_admin.html` | Árak, felárak, kiegészítők beállítása + beérkezett foglalások | csak belső |
| `arajanlat_keszito.html` | Foglalásból (vagy kézzel) nyomtatható árajánlat | csak belső |

A `robots.txt` és a `noindex` meta tiltja a keresőmotorokat, tehát a Google nem
listázza az oldalt – aki nem kapja meg a linket, nem botlik bele.

## Fontos: az adatok a böngészőben élnek

Nincs szerver és nincs adatbázis. Az árlista és a beérkezett foglalások a böngésző
saját tárolójában (localStorage) vannak, gépenként és böngészőnként külön.

- Az adminban beállított árakat **csak abban a böngészőben** látod, ahol beállítottad.
- A látogatók a fájlba épített alapárakat látják.
- Átvitel másik gépre: admin → **⬇ Export JSON**, a másik gépen **⬆ Import JSON**.
- Tartós áremeléshez a `arkalkulator_foglalas.html` `DEFAULTS` részét kell frissíteni.

## Frissítés (fejlesztői teendő)

Ez a mappa **generált** – ne itt szerkessz! A források a fő projektben vannak:

```
Árkalkulátor/arkalkulator_admin.html
Árkalkulátor/arkalkulator_foglalas.html
Árajánlat/arajanlat_keszito.html
```

Változás után a projekt gyökerében:

```powershell
powershell -ExecutionPolicy Bypass -File .\github_frissites.ps1
```

majd:

```bash
git add -A && git commit -m "Frissítés" && git push
```

Az `index.html`, a `README.md` és a `robots.txt` viszont csak itt él – azokat a
szkript nem írja felül.
