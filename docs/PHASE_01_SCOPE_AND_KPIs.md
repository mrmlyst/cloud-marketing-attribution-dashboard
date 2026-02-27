# Project: Cloud Marketing Attribution & Campaign Lift Dashboard
## Phase 1 — Scope & KPI Definitions

### Project Story
An e-commerce company runs marketing campaigns across multiple channels (paid search, paid social, email, referral, organic). This project builds a cloud analytics workflow to measure channel performance, customer funnel behavior, and campaign impact to optimize spend and revenue.

---

### Business Questions
1. Which channels drive the most revenue and orders?
2. Which channels convert best from session → purchase?
3. Where are the biggest funnel drop-offs (view → add_to_cart → purchase)?
4. What is marketing efficiency by channel (ROAS, CAC)?
5. Did a campaign launch improve performance (pre vs post)?
6. Which channels acquire the highest-value customers?

---

### Funnel Definition (Events-Based)
We will model a funnel using event logs:
- View event: `view`
- Add to cart event: `add_to_cart`
- Purchase event: `purchase`

Key funnel metrics:
- Views
- Add-to-cart
- Purchases
- View → Add-to-cart rate
- Add-to-cart → Purchase rate
- View → Purchase conversion rate

---

### KPI Dictionary (Definitions & Formulas)

#### Revenue & Orders
- Revenue = SUM(order_total)
- Orders = COUNT(DISTINCT order_id)
- AOV (Average Order Value) = Revenue / Orders

#### Traffic & Funnel
- Sessions = COUNT(DISTINCT session_id)
- Views = COUNT(*) WHERE event_type = 'view'
- Add-to-cart = COUNT(*) WHERE event_type = 'add_to_cart'
- Purchases (events) = COUNT(*) WHERE event_type = 'purchase'
- View → Add-to-cart rate = add_to_cart_events / view_events
- Add-to-cart → Purchase rate = purchase_events / add_to_cart_events
- View → Purchase conversion rate = purchase_events / view_events

#### Marketing Efficiency
- Spend = SUM(spend)
- ROAS = Revenue / Spend
- New Customers = COUNT(DISTINCT customer_id) where first_purchase_date in period
- CAC = Spend / New Customers

#### Customer Value
- Lifetime Revenue per customer = SUM(order_total) by customer_id
- High-value customers = top 20% of customers by lifetime revenue (NTILE(5)=1)

#### Campaign Impact (Lift)
- Campaign launch date: (to be set from campaigns table)
- Before window: 30 days before launch
- After window: 30 days after launch
- Revenue Before = revenue in before window
- Revenue After = revenue in after window
- Lift % = (After - Before) / Before

---

### Dimensions (How we slice results)
- Time (day/week/month)
- Channel (paid_search, paid_social, email, referral, organic, direct)
- Campaign (campaign_id)
- Device (mobile/desktop)
- Customer segment (new vs returning; high-value vs others)

---

### Data Requirements (Tables & Required Columns)

1) customers
- customer_id
- signup_date (optional)

2) sessions
- session_id
- customer_id (nullable)
- session_date
- channel
- device

3) events
- event_id (or generated)
- session_id
- customer_id (nullable if unknown)
- event_time
- event_type (view, add_to_cart, purchase)
- product_id (optional)

4) orders
- order_id
- customer_id
- order_date
- order_total
- session_id (optional but recommended)

5) campaigns
- campaign_id
- channel
- campaign_name
- start_date
- end_date
- launch_date (or use start_date as launch_date)

6) ad_spend
- date
- channel
- campaign_id (optional)
- spend

---

### Definition of Done (Phase 1 Complete)
- [ ] Project story written
- [ ] Business questions listed
- [ ] Funnel definition agreed (events-based)
- [ ] KPI dictionary with formulas completed
- [ ] Campaign window assumptions defined
- [ ] Dimensions listed
- [ ] Data requirements (tables + columns) defined
