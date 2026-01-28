# Bestseed.eu

**OVH és OneProvider szerverek kezelése webes felületen**

---

## Főbb funkciók

- OVH dedikált és VPS szerverek kezelése  
- OneProvider szerverek integrációja  
- IP (IPv4) és IPv6 címek lekérése és megjelenítése  
- Státusz (running, ok, stopped) megjelenítése  
- Start / Stop / Reboot / Reinstall gombok  
- Felhasználókhoz szerver hozzárendelés admin felületen  
- Gyorsítótár (Memcached) a lekérésekhez  

---

## Technológiai áttekintés

- **Backend:** PHP 8.4, MVC alapú architektúra  
- **API integrációk:**  
  - OVH API (dedikált és VPS szerverekhez) – OVH PHP SDK  
  - OneProvider API (felhő- és VPS szerverekhez)  
- **HTTP kliens:** Guzzle a REST API hívásokhoz  
- **Cache:** Memcached a szerverlisták gyorsítótárazásához  
- **Frontend:** Bootstrap 5, reszponzív kártya nézet a szerverekhez, gombok a műveletekhez  
- **Jogosultságkezelés:** Adminok teljes hozzáféréssel, felhasználókhoz rendelt szerverekhez korlátozott hozzáférés  

---

## Működés

- A dashboard lekéri az OVH és OneProvider szerverek adatait API-n keresztül.  
- A szerveradatok gyorsítótárazása Memcached segítségével történik, a cache csak akkor frissül, ha:  
  - új szerver kerül az API-ba  
  - egy szerver új felhasználóhoz kerül hozzárendelve  
  - egy szerver állapotát manuálisan frissítik (restart, stop, start)  
- A root jelszavak csak azoknak láthatók, akik jogosultak (admin vagy szerverhez rendelt felhasználó).  

---

## Notes / Known Issues

- OVH VPS start/reboot műveleteknél előfordulhat, hogy a VPS nem indul el azonnal, ha a státusz még `stopping` állapotban van.  
  - Megoldás: a `dashboard.php` POST handlerben a `try/catch` blokkok biztosítják, hogy az API hibák feedbackként jelenjenek meg.  
- A `panel.js`-t cache-ből mindig frissíteni kell:  

html
<script src="/js/panel.js?v=<?= time() ?>"></script>
