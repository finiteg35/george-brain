# Vapi Phone Agent (phone.everblack.cloud)

## Purpose
Inbound call ordering — mirrors web app experience exactly (same email confirmation, same API).

## Vapi Config
- **Private key:** 0900fa82-8e7b-4d0f-b6c7-3739625f0fdf
- **Assistant ID:** eaed174c-7080-4ac9-b9a1-28e7c651e806
- **Phone number ID:** b83034f6-a6b3-459c-ace2-7904d42f2e3e
- **Model:** gpt-5-mini (set in Vapi dashboard — confirm exact name)
- **waitSeconds:** 1.5

## Live Tools
All hitting orders.everblack.cloud with produce API key:
- `get_stores` — fetch store list
- `get_inventory` — fetch available products
- `add_order_item` — place line items
- `send_confirmation` — trigger confirmation email

## Order Requirements
- Must include `vendor_id="gmf"`
- Shared `submitted_at` timestamp across all items in one call
- `store_name` = display name (not username) for correct grouping on orders screen

## Next Steps
- Confirm model name (gpt-5-mini vs gpt-4o-mini)
- Test full end-to-end order submission
