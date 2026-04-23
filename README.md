# Zuma Stok per Area

Standalone dashboard agregasi stok per zona logistik (Bali · Jakarta · Jatim) — gabungan Warehouse + Toko Retail + Consignment.

## URL
- Production: https://database-zuma.github.io/zuma-stockbyarea-dashboard/

## Stack
- Vanilla HTML + CSS + JS (single-file, ~42KB)
- Backend: shared API `srv1346756.hstgr.cloud` (FastAPI + PostgreSQL)
- Auth: `X-API-Key` header
- Deploy: GitHub Pages (auto from `main`)

## Features
- Toggle entity: DDD · MBB · UBB · LJBB
- Toggle area (zona logistik):
  - **Bali** = Bali + Lombok
  - **Jakarta** = Jakarta
  - **Jatim** = Jatim + Sulawesi + Sumatra + Batam (supplied from WH Pusat Surabaya)
- Filter tier, version, search by kode/nama
- Toggle kolom: Warehouse / Retail / Consignment
- Color-code per qty: 🔴 ≤0 · 🟡 1-5 · 🟢 ≥6
- Export CSV

## API endpoints used
- `/api/stock` — data stok gabungan warehouse + retail per entity
- `/api/store-areas` — mapping toko → area
- `/api/stock/consignment` — stok consignment

## Dev
```
python3 -m http.server 8000
# buka http://localhost:8000
```

## Source
Dipisahkan dari `stock-inventory-dashboard/dashboard_inventory.html` (lihat `CLAUDE.md` untuk detail).
