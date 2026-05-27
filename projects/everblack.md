# Everblack Platform

## Architecture
```
Internet
    ├── orders.everblack.cloud → produceorderapp container (port 8000)
    └── watcherhq.net          → watcherhq container
                                      └── Internal API → produceorderapp
```

## VPS
- Provider: Hostinger KVM 2, Boston US
- IPv4: 187.124.248.154 | IPv6: 2a02:4780:2d:aa82::1
- OS: Ubuntu 24.04 LTS | CPU: 2 vCPUs | RAM: 8 GB | Disk: 100 GB (~25 GB used)
- SSH: root access | Auto-renewal: on | Expires: 2027-04-01

---

## Produce Order App (orders.everblack.cloud)

### Key Paths
- **Live app.py:** `/docker/openclaw-dtsu/data/.openclaw/workspace/produceorderapp/app.py` (volume-mounted)
- **Host copy:** `/root/produceorderapp/app.py`
- **Container:** `produceorderapp-produceorderapp-1`
- **Data (container):** `/data/` — orders.json, users.json, inventory.json, qr_tokens.json
- **API Key:** `openclaw-produce-api-key-2026`
- **Env vars:** `/root/produceorderapp/.env`

### Deploy
```bash
docker cp /root/produceorderapp/app.py produceorderapp-produceorderapp-1:/app/app.py && docker restart produceorderapp-produceorderapp-1
```
Then commit to finiteg35/everblack.

### Data Model
- `orders.json` = individual LINE ITEMS, one record per item ordered
- Orders group by: `store_name|delivery_date|ordered_by|submitted_at[:16]`
- NEVER use `/api/users` for write ops — strips passwords. Read/write container JSON directly.

### GMF / Vendor
- `vendor_id = "gmf"` = house account — always Pro, exempt from store limits
- Office email: ethan@gmfproduce.com
- Warehouses: Brewer (88 Stevens Rd, Brewer ME), Biddeford (415 Hill St, Biddeford ME)

### Plan Tiers
| Feature | Starter | Standard | Pro | Seasonal |
|---|---|---|---|---|
| Orders + catalog | ✅ | ✅ | ✅ | ✅ |
| Invoicing + PDF | ❌ | ✅ | ✅ | ✅ |
| QuickBooks sync | ❌ | ✅ | ✅ | ✅ |
| Auto-invoice / Auto-QB | ❌ | ✅ | ✅ | ✅ |
| Barcode/UPC | ❌ | ❌ | ✅ | ❌ |
| Up to 30 stores | ❌ | ❌ | ✅ | ❌ |

### Routes
- **Brewer (33 stores):** hannaford_airport, hannaford_barharbor, hannaford_bluehill, hannaford_brewer, hannaford_broadway, hannaford_bucksport, hannaford_ellsworth, hannaford_hampden, hannaford_hoganroad, hannaford_lincoln, hannaford_oldtown, chases_restaurant, dennis_food, edward_brothers, friends_family, gm_familymarket, hilton_garden_inn, marsh_island, masons_brewing, paddy_murphys, anglers_hampden, anglers_newport, anglers_searsport, dysarts_broadway, dysarts_hermon, governors_oldtown, governors_presqueisle, governors_waterville, danforths_market, danforths_downhome, lincoln_country, brewer_iga, edwards_shopnsave
- **Biddeford (31 stores):** hannaford_biddeford, hannaford_saco, hannaford_scarborough, hannaford_mainemall, hannaford_millcreek, hannaford_riverside, hannaford_falmouth, hannaford_westbrook, hannaford_forestave, hannaford_kennebunk, hannaford_northberwick, hannaford_sanford, hannaford_wells, hannaford_yarmouth, hannaford_york, hannaford_scarborough_payne, marketbasket_biddeford, marketbasket_westbrook, marketbasket_topsham, beach_lobster, beckys_diner, josephs_bythesea, native_maine, pier_fries, ramunos, robins_confections, rosemont_bakery, scratch_bakery, twofatcats, valeries_diner

### Password Conventions
- Store accounts: `{username}2026` (strip chain prefix, e.g. hannaford_westbrook → westbrook2026)
- Driver (eddie): `eddie2026` | Vendor (greenmeadow): `gmf2026`
- QR code login preferred for stores — takes directly to catalog, no password needed
- ⚠️ NEVER write users.json without preserving password fields

### Cutoff System
- Default: 13:30 ET — past cutoff + delivery = tomorrow → amber warning + late flag in email
- Cron job `produce-cutoff-summary` runs 17:30 UTC daily

### Inventory
- 116 Hannaford items (vendor_id=gmf) — ONLY from GMF 197 scan sheet
- Hannaford stores see Hannaford inventory only

### QuickBooks
- App: Everblack Orders | Redirect: https://orders.everblack.cloud/qb/callback
- QB tokens on `greenmeadow` user record: qb_token, qb_refresh_token, qb_realm_id
- 38 stores have qb_customer_name set | beckys_diner has no QB match

---

## WatcherHQ (watcherhq.net)

### Key Paths
- **App:** `/docker/openclaw-dtsu/data/.openclaw/workspace/watcherhq/app.py`
- **Container:** `watcherhq`
- **Deploy:** `docker cp .../watcherhq/app.py watcherhq:/app/app.py && docker restart watcherhq`

### Purpose
Vendor-facing SaaS platform for Everblack Orders. Vendors sign up, subscribe via Stripe, get a produce app account linked via `produce_vendor_id`.

### Database (SQLite)
- `/data/watcherhq.db` in container
- Tables: users, vendor_feed, ai_usage, _migrations

### Pricing
| Plan | Price | Stores | Setup |
|---|---|---|---|
| Starter | $99/mo | 10 | $199 |
| Standard ★ | $199/mo | 10 | $299 |
| Pro | $349/mo | 30 | $399 |
| Seasonal | $999/season | 10 | $299 |

### Vendor Accounts
- **GMF:** ethan@gmfproduce.com / gmf2026 / Pro / comped=1 / produce_vendor_id=gmf
- **Everblack (George):** everblack@watcherhq.net / Pro / comped=1 / produce_vendor_id=gmf
- `comped=1` → skips Stripe, shows "Managed account"
- Admin: `/admin` — everblack@watcherhq.net only

### Stripe (test keys — NOT live)
- Pub: `pk_test_51TJH6MQuwA34OdBQqI3oOhjKzQ1CO805hWHLvkFMGfBW3ZqQLT1Cp3HWl3AbdUY5Hw0owBTLK9DomPLW5KNniqtT00qhIsDo4s`
- Next: Create 4 products in Stripe prod, add price IDs to `/root/watcherhq/.env`, configure webhook

### Email
- SMTP: smtp.hostinger.com:465, user: everblack@watcherhq.net
- Flows: new vendor alert, vendor welcome, contact form, vendor-to-vendor contact
