# Zuma Stok per Area — Agent Context

Standalone dashboard (di-spawn dari `stock-inventory-dashboard`). Cuma view Stok per Area — agregasi WH + Toko + Consignment per zona logistik.

## Architecture

```
Browser (GitHub Pages)
  └─ index.html (single-file HTML+CSS+JS, no external libs)
       └─ fetchAPI() → https://srv1346756.hstgr.cloud (VPS)
            └─ nginx → gunicorn → FastAPI → PostgreSQL (openclaw_ops)
```

Shared backend dengan `stock-inventory-dashboard` dan `zuma-sales-dashboard`. API key sama.

## Key constants (index.html)

- `API_BASE` = `https://srv1346756.hstgr.cloud`
- `API_KEY` = `97d25067-a2ca-44ba-ac5b-61539b627271`
- Deploy: `https://database-zuma.github.io/zuma-stockbyarea-dashboard/`

## API endpoints used

- `/api/stock` → populates `allData[entity].warehouse` + `allData[entity].retail`
- `/api/store-areas` → populates `storeAreaMap` (store name → area name)
- `/api/stock/consignment` → populates `consignmentStockItems`

Tidak butuh endpoint lain. Jauh lebih ringan dari master dashboard (yang fetch 10 endpoint + Phase 2 monthly detail).

## Zone mapping (penting — jangan ubah tanpa konfirmasi)

```
Bali zone    = WH Bali, WH Lombok, Mataram, plus toko/consig area "Bali", "Lombok"
Jakarta zone = WH Pluit, WH Jakarta, plus toko/consig area "Jakarta", "Online"
Jatim zone   = WH Pusat Surabaya, WH Malang, WH Makassar, WH Manado, WH Medan, WH Batam,
               plus toko/consig "Jatim", "Sulawesi", "Sumatra", "Batam"
```

Logic ada di `sbaWhToArea()` dan `sbaStoreMapArea()`.

## State & rendering flow

```
DOMContentLoaded
  → loadDashboardData() fetches 3 endpoints in parallel
  → initStockByArea() builds area tabs + populates filter dropdowns
  → renderStockByArea() renders table for sbaCurrentArea

User switches entity:
  → selectEntity() → populateSbaFilters() → renderStockByArea()

User switches area:
  → switchSbaArea() → renderStockByArea()

User types search / changes filter:
  → renderStockByArea() (filtering in-memory, no re-fetch)
```

## Source of truth

Disalin dari `/Users/database-zuma/stock-inventory-dashboard/dashboard_inventory.html`:
- HTML section `#stockbyareaView` (~line 2670)
- JS functions `sbaWhToArea`, `sbaStoreMapArea`, `sbaIsConsigArea`, `initStockByArea`, `switchSbaArea`, `populateSbaFilters`, `buildStockByAreaRows`, `buildSbaColumns`, `renderStockByArea`, `exportStockByAreaCSV` (~line 16300-16749)

Kalau logic di master berubah, **sync manual** ke sini.

## Safe edit rules

- Vanilla JS only (no bundler, no framework)
- Always test in browser before committing
- CORS locked to `https://database-zuma.github.io` on VPS nginx
- Don't change zone mapping without asking

## Git

```
cd /Users/database-zuma/zuma-stockbyarea-dashboard
git add index.html
git commit -m "feat: ..."
git push origin main
```

GitHub Pages auto-deploys from `main`.
