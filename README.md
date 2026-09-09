# Deeply – Árkalkulátor és árajánlat-készítő

Ideiglenes munkafelület a Deeply (kárpit- és szőnyegtisztítás) belső eszközeihez.
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

Nincs szerver és nincs adatbázis. Az árlista, a beérkezett foglalások és a mentett
ajánlatok a böngésző saját tárolójában (localStorage) vannak, gépenként és
böngészőnként külön. A GitHub Pages csak a *programot* szolgálja ki, adatot nem tárol.

Ezért van a teljes adatmentés – az adminban a fejlécben, az árajánlat-készítőben a
`💾 Adatmentés` gomb alatt:

| Gomb | Mit csinál |
|---|---|
| 💾 Teljes mentés | Minden adat egy tömör JSON-fájlba: árlista + foglalások + ajánlatok + sorszám. Behúzás nélküli JSON = a lehető legkisebb fájl; ha vannak fotók, felajánlja a fotó nélküli (jóval kisebb) mentést. |
| 📥 Visszatöltés | Beolvassa a mentést és **összefésüli** a meglévővel – nem töröl. A hiányzó tételek hozzáadódnak, azonos ajánlatszámnál a frissebb marad. Így két gép adata egyesíthető. |
| ⬇ Árak exportja / ⬆ Árak importja | Csak az árlista, ember által is olvasható formában (a beépített alapárak frissítéséhez). |

Javasolt szokás: **munkanap végén egy Teljes mentés** a Drive-ba vagy egy mappába.

## Átvitel a jövőbeli szerverre

A Teljes mentés formátuma szándékosan gépi feldolgozásra készült, hogy egy későbbi
weboldal vagy adatbázis be tudja olvasni:

```json
{
  "format": "deeply-backup",
  "version": 1,
  "exportedAt": "2026-09-09T10:00:00.000Z",
  "data": {
    "pricing":  { "services": [], "materials": [], "settings": {} },
    "bookings": [ { "created": 1757000000000, "customer": {}, "items": [] } ],
    "offers":   [ { "no": "DPL-2026-001", "savedAt": 1757000000000, "items": [] } ],
    "offerSeq": { "year": 2026, "n": 1 }
  }
}
```

- A `bookings[].created` (időbélyeg) és az `offers[].no` (ajánlatszám) az egyedi
  azonosító – ezek mentén megy az összefésülés, és ezek lehetnek a jövőbeli
  adatbázis kulcsai is.
- A foglalás tételei bontva vannak (mennyiség, egységár, anyag, foltok,
  kiegészítők), tehát nem kell visszafejteni a szöveges címkékből.
- A dátumok ISO-formátumúak, az összegek számok (nem formázott szöveg).
- A `version` mező miatt egy későbbi formátumváltás is kezelhető marad.

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

A teljes adatmentés kódja (`bkExport` / `bkImport`) **két fájlban** él:
`arkalkulator_admin.html` és `arajanlat_keszito.html` – módosításkor mindkettőt
át kell vezetni.
