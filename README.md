# 🛒 Shopping List Plus

Integrácia nákupného zoznamu pre Home Assistant s vlastným panelom, kartou pre dashbaord, skenerom čiarových kódov a podporou produktov s variantmi.

---

## Funkcie

- **Vlastný panel** — plnohodnotný správca nákupného zoznamu priamo v HA
- **Dashboard karta** — kompaktná karta pre dashboard
- **Skener čiarových kódov** — cez kameru mobilu (Companion app)
- **Produkty s variantmi** — umožňuje vyplniť rôzne varianty produktu, rôznych výrobcov rovnakého produktu, kusy, objem alebo urobiť skupinu podobných produktov, napr. "Kuriacie mäso" varianty - "Krídla", "Stehná"
- **Obľúbené položky** — rýchly prístup k často kupovaným produktom
- **Kategórie** — farebné kategórie s drag & drop zoradením
- **Nákupný režim** — zjednodušené zobrazenie dashboard karty pri nakupovaní - ručne alebo automatizáciou
- **Vlastné ikony** — PNG/SVG ikony pre produkty

---

## Požiadavky

- Home Assistant 2024.1.0 alebo novší
- [Home Assistant Companion App](https://companion.home-assistant.io/) (pre skener a notifikácie)

---

## Inštalácia

### Manuálna inštalácia

1. Stiahni poslednú verziu z [Releases](https://github.com/GegoSK/shopping-list-plus/releases)
2. Skopíruj `custom_components/shopping_list_plus/` do `/config/custom_components/`
3. Skopíruj `www/community/shopping-list-card/` do `/config/www/community/`
4. Reštartuj Home Assistant
5. Pridaj integráciu: **Nastavenia → Zariadenia & služby → Pridať integráciu → Shopping List Plus**

### Cez HACS

1. Otvor HACS → **Vlastné repozitáre**
2. Pridaj URL: `https://github.com/GegoSK/shopping-list-plus`
3. Kategória: **Integrácia**
4. Nainštaluj **Shopping List Plus**
5. Reštartuj Home Assistant

---

## Konfigurácia

Pri pridávaní integrácie vyplň:

| Pole | Popis | Default |
|------|-------|---------|
| **Nákupný zoznam** | HA `todo` entita | — |
| **Webhook ID** | ID pre príjem čiarových kódov | `shopping_barcode_scan` |
| **Súbor produktov** | Cesta k JSON databáze | `/config/shopping_favorites.json` |
| **Priečinok ikon** | Cesta k vlastným ikonám | `/config/custom_components/shopping_list_plus/icons` |
| **Notify služba** | Companion app služba pre TTS/vibrácie | — |

---

## Dashboard karta

Pridaj kartu do dashboardu:

```yaml
type: custom:shopping-list-card
todo_entity: todo.nakupny_zoznam
```

### Možnosti karty

| Možnosť | Popis | Default |
|---------|-------|---------|
| `todo_entity` | HA todo entita (povinné) | — |
| `favorites_file` | Cesta k JSON súboru | `/config/shopping_favorites.json` |
| `product_size` | Výška tlačidiel `s/m/l` | `m` |
| `icon_size` | Veľkosť ikon `s/m/l` | `m` |
| `grid_columns` | Počet stĺpcov produktov | `4` |
| `purchase_delay` | Oneskorenie presunu kúpených (s) | `3` |
| `variant_select_delay` | Čas na výber variantov (s) | `3` |
| `accent_color` | Farba zvýraznenia | — |
| `show_cart_icon` | Ikona košíka pri kúpenom | `true` |
| `show_icons_in_list` | Ikony v nákupnom zozname | `false` |
| `barcode_webhook` | Webhook ID pre skener | `shopping_barcode_scan` |

---

## Skener čiarových kódov

Integrácia prijíma čiarové kódy cez webhook. Companion app posiela POST request:

```json
{
  "barcode": "1234567890",
  "name": ""
}
```

Ak je čiarový kód známy — produkt sa pridá do nákupného zoznamu.
Ak nie — uloží sa do **Pending** zoznamu na neskoršie priradenie.

### Nastavenie v Companion app

Vytvor automatizáciu v Companion app ktorá posiela naskenovaný kód na:
```
https://[HA_URL]/api/webhook/shopping_barcode_scan
```

---

## Odozva pri skenovaní

Nastaviteľné v paneli → **Nastavenia**:

| Typ | Popis |
|-----|-------|
| **Zvuk** | Web Audio — priamo v karte/paneli |
| **Vibrácia** | Cez Companion app notifikáciu |
| **TTS** | Hlasové oznámenie cez Companion app |

Samostatne nastaviteľné pre **nájdený** a **neznámy** čiarový kód.


---

## Licencia

MIT License — voľné použitie a úpravy.
