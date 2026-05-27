# Everblack Platform

## Produce Order App
- **Live app.py path:** `/docker/openclaw-dtsu/data/.openclaw/workspace/produceorderapp/app.py`
- **Container:** `produceorderapp-produceorderapp-1`
- **API:** orders.everblack.cloud
- **Status:** Feature-complete as of May 2026

### Data Model
- `orders.json` stores individual line items — one record per item ordered
- "Orders" = full cart submissions; "line items" = individual records
- Orders group by: `store_name|delivery_date|ordered_by|submitted_at[:16]`

### Order Requirements
- Must include `vendor_id="gmf"`
- Shared `submitted_at` timestamp across all items in one cart submission
- `store_name` = display name (not username) for correct grouping
