# Medium-Scale Food Delivery Synthetic Dataset (57 Tables)

This dataset simulates a full food-delivery ecosystem with **57 fully populated relational tables**, **10–40 events per trace**, and **two deterministic branching points**.

The dataset is designed for:

- Event log reconstruction  
- Schema-agnostic join inference  
- Temporal ordering reconstruction  
- Multi-hop key propagation  
- Robustness experiments (schema drift + timestamp missingness)  

---

# ⭐ Highlights

- **57 populated tables**
- **10–40 events per trace**
- **Strict timestamp monotonicity**
- **Two branching points**  
  - Branch A → *2 possible flows* (Risk outcome)  
  - Branch B → *4 possible flows* (Delivery exception)  
- **All tables have ≥1 timestamp**
- Many tables have:
  - `created_ts`
  - `updated_ts`
  - `batch_run_ts`
  - `effective_ts`
- Fully deterministic processes (no randomness-driven branching)
- Perfect for benchmarking complex reconstruction tasks

---

# 🧩 Event Flow Summary

## Branch A (2-way) — Risk Decision  
From `RiskCheckCompleted`, flow branches based on:
- `risk_scores.risk_level`

Paths:
- `AutoApprovalGranted`
- `ManualReviewRequired` → `ManualReviewCompleted`

---

## Branch B (4-way) — Delivery Exception  
From `DeliveryExceptionOccurred`, flow branches based on:
- `delivery_exceptions.exception_type`

Possible paths:
- `REROUTE`
- `DELAY`
- `REASSIGN_DRIVER`
- `CANCEL`

Each results in a different sequence of subsequent events.

---

# 📂 Dataset Contents

Output includes:

- `medium_event_traces.csv`  
- `medium_transition_graph.csv`  
- **57 CSV tables under `/data_medium/`**, covering:

### Customer Lifecycle  
- customers  
- customer_addresses  
- customer_devices  
- customer_payment_methods  
- customer_verification  
- customer_loyalty_status

### Merchant & Menu System  
- merchants  
- merchant_locations  
- merchant_business_docs  
- merchant_bank_accounts  
- merchant_operational_status  
- menu_items  
- menu_item_options  
- inventory_reservations  
- inventory_adjustments  
- merchant_menu_updates

### Drivers & Logistics  
- drivers  
- driver_shift_status  
- driver_location_updates  
- route_plans  
- route_segments  
- traffic_incidents

### Ordering & Processing  
- orders  
- order_items  
- order_discounts  
- order_status_events  
- order_tax_calculation  
- order_promo_validation  
- order_cart_snapshots  
- order_experiments

### Risk & Fraud  
- risk_requests  
- risk_scores  
- fraud_checks  
- manual_review_cases  

### Fulfillment  
- kitchen_tickets  
- kitchen_events

### Dispatch & Delivery  
- dispatch_requests  
- driver_assignments  
- delivery_status_events  
- delivery_package_scans  
- handover_confirmations  
- delivery_exceptions  
- reassignment_requests  
- fallback_delivery_methods

### Payments & Refunds  
- payment_authorizations  
- payment_captures  
- payment_settlements  
- refund_requests  
- refund_approvals  
- refund_settlements  

### Support & Engagement  
- support_tickets  
- support_ticket_messages  
- support_issue_resolutions  
- customer_reviews  
- customer_rewards

### Audit & Actions  
- system_audit_logs  
- user_action_logs

---

# 🧪 Noise Injection (Schema Drift + Missing Timestamps)

Noise injector is provided as `noise_injector_medium.py`.

It applies:

### **1. 30% Attribute Swap (Schema Drift)**
- Renames 30% of safe columns to `attr_XXX`
- Adds type drift
- Swaps values between non-key columns
- Preserves all:
  - PKs
  - FKs
  - Join keys
  - Event logs

### **2. 15% Timestamp Missingness**
- Randomly removes timestamps from all event/dimension tables
- Preserves event trace ordering
- Leaves `medium_event_traces.csv` untouched

Run:

python noise_injector_medium.py

yaml
Copy code

Result is saved in-place into `/data_medium/`.

---

# 📜 License  
Free for research, academic use, and benchmarking.