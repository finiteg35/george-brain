# Vapi Phone Agent

- **URL:** phone.everblack.cloud
- **Purpose:** Inbound call ordering — mirrors web app experience exactly (same email confirmation, same API)
- **Provider:** Vapi
- **Private key:** 0900fa82-8e7b-4d0f-b6c7-3739625f0fdf
- **Assistant ID:** eaed174c-7080-4ac9-b9a1-28e7c651e806
- **Phone number ID:** b83034f6-a6b3-459c-ace2-7904d42f2e3e
- **Model:** gpt-5-mini (set in Vapi dashboard)
- **waitSeconds:** 1.5

## Live Tools
- `get_stores` — fetch store list
- `get_inventory` — fetch available products
- `add_order_item` — place line items
- `send_confirmation` — trigger confirmation email

All tools hit orders.everblack.cloud with produce API key.

## Next Steps
- Confirm model name (gpt-5-mini vs gpt-4o-mini)
- Test full end-to-end order submission
