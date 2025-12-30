# Project Clarifications

> Please review and select options or provide input for each question below. These decisions are required before proceeding to roadmap definition and feature decomposition.

---

## Q1: Product Inventory Authority and Synchronization

**Context:** The PRD mentions "Assumes external inventory system integration" in Out of Scope, but doesn't clarify inventory data ownership and real-time synchronization requirements.

**Decision Required:**

- [ ] **Option A**: Product inventory is managed externally; itsme.fashion reads inventory via API at checkout only (accept lag risk)
- [x] **Option B**: Product inventory is synchronized to Firestore in near-real-time (< 5 seconds); itsme.fashion owns the read model
- [ ] **Option C**: Product inventory is fully owned by itsme.fashion; external system synchronizes from us
- [ ] **Other**: [Please specify the inventory authority model]

**Impact:** This affects data architecture, overselling risk mitigation, and cart/checkout flow design.

---

## Q2: Cart Persistence for Unauthenticated Users

**Context:** The PRD mentions "session persistence" for carts but doesn't clarify whether unauthenticated user carts persist across sessions or devices.

**Decision Required:**

- [ ] **Option A**: Unauthenticated carts are session-only (cleared when session ends)
- [ ] **Option B**: Unauthenticated carts persist via browser localStorage across sessions on the same device
- [x] **Option C**: Unauthenticated carts are assigned a guest identifier and persist server-side for 30 days
- [ ] **Other**: [Please specify cart persistence behavior]

**Impact:** This affects user experience, data storage requirements, and cart recovery strategies.

---

## Q3: Payment Failure Recovery and Retry Handling

**Context:** The PRD lists payment failure as a domain event but doesn't specify user-facing behavior or system recovery strategy.

**Decision Required:**

When a payment fails during checkout, should the system:

- [x] **Option A**: Allow the user to retry immediately with the same payment method
- [ ] **Option B**: Allow the user to retry with a different payment method only
- [ ] **Option C**: Require the user to re-initiate checkout from the cart (order is cancelled)
- [ ] **Option D**: Hold the order in "Payment Pending" state for 24 hours with retry link sent via email
- [ ] **Other**: [Please specify payment failure handling]

**Impact:** This affects order lifecycle, user trust, inventory reservation duration, and support burden.

---

## Q4: Order Cancellation and Refund Window

**Context:** The PRD doesn't address whether users can cancel orders post-payment and before shipment.

**Decision Required:**

- [ ] **Option A**: Users cannot cancel orders after payment is completed (final sale)
- [x] **Option B**: Users can cancel orders until shipment is created (full refund)
- [ ] **Option C**: Users can cancel orders within a fixed time window (e.g., 1 hour) after placement
- [ ] **Option D**: Users can request cancellation via support; approval required
- [ ] **Other**: [Please specify cancellation policy]

**Impact:** This affects order state machine design, refund workflows, user trust, and operational complexity.

---

## Q5: Wishlist Behavior Across Devices

**Context:** Wishlist is described as requiring authentication but synchronization behavior isn't specified.

**Decision Required:**

- [ ] **Option A**: Wishlist is device-specific (stored locally); no cross-device sync
- [x] **Option B**: Wishlist is user-specific and syncs across all authenticated sessions
- [ ] **Option C**: Wishlist syncs across devices but is capped at a maximum item count (e.g., 50 items)
- [ ] **Other**: [Please specify wishlist synchronization behavior]

**Impact:** This affects data storage, sync complexity, and user experience expectations.

---

## Q6: Product Availability Notification

**Context:** The PRD doesn't address what happens when a user adds an out-of-stock or low-stock product to their wishlist or cart.

**Decision Required:**

For out-of-stock products, should the system:

- [ ] **Option A**: Prevent adding to cart; allow adding to wishlist with no notification
- [x] **Option B**: Prevent adding to cart; allow adding to wishlist with email notification when back in stock
- [ ] **Option C**: Allow adding to cart but show clear "out of stock" messaging; notify when available
- [ ] **Other**: [Please specify out-of-stock handling]

**Impact:** This affects user frustration, notification infrastructure requirements, and inventory integration.

---

## Q7: Guest Checkout Support

**Context:** The PRD describes authentication as a core feature but doesn't explicitly state whether guest checkout is supported.

**Decision Required:**

- [ ] **Option A**: Guest checkout is supported; users provide email and shipping info without account creation
- [x] **Option B**: Guest checkout is supported but users are encouraged to create account post-purchase
- [ ] **Option C**: Account creation is required before checkout (no guest checkout)
- [ ] **Other**: [Please specify guest checkout policy]

**Impact:** This affects conversion rates, user friction, authentication architecture, and order history management.

---

## Q8: Data Residency and Geographic Restrictions

**Context:** The PRD mentions GDPR and CCPA compliance but doesn't specify data residency requirements or geographic availability.

**Decision Required:**

- [ ] **Option A**: Single global deployment; data stored in one GCP region (specify: [region])
- [ ] **Option B**: Multi-region deployment with data residency enforcement (EU data in EU, etc.)
- [x] **Option C**: Single region deployment with geographic access restrictions (e.g., US-only initially) - Region: us-central1
- [ ] **Other**: [Please specify data residency and geographic scope]

**Impact:** This affects infrastructure architecture, compliance complexity, and go-to-market scope.

---

## Q9: Email Notification Ownership

**Context:** The PRD mentions "Email notifications via Firebase extensions" but doesn't clarify the scope of automated emails.

**Decision Required:**

Which email notifications are automated vs. manual/support-driven?

- [ ] **Option A**: Automated emails for: order confirmation, shipment tracking, payment failure only
- [x] **Option B**: Automated emails for: order confirmation, shipment tracking, payment failure, abandoned cart, wishlist back-in-stock
- [ ] **Option C**: Automated emails for all user-facing events including marketing (product recommendations, promotions)
- [ ] **Other**: [Please specify email notification scope]

**Impact:** This affects infrastructure requirements, email deliverability management, and user communication strategy.

---

## Q10: Real-Time vs. Eventual Consistency Tolerance

**Context:** The PRD specifies "Eventual consistency < 1s" but doesn't clarify which operations require strict consistency.

**Decision Required:**

Which operations require strong (immediate) consistency vs. eventual consistency?

**Strongly consistent operations:**
- [x] Payment processing
- [x] Order creation
- [x] Inventory deduction at checkout
- [ ] [Other - please specify]

**Eventually consistent operations:**
- [x] Product catalog updates
- [x] Wishlist synchronization
- [x] Order status updates (except payment-related)
- [ ] [Other - please specify]

**Impact:** This affects database design, performance characteristics, and user experience edge cases.

---

## Q11: Ethical Marker Verification and Trust

**Context:** The PRD emphasizes ethical markers (cruelty-free, paraben-free, vegan) as a core value proposition but doesn't specify verification or certification requirements.

**Decision Required:**

How are ethical marker claims verified?

- [ ] **Option A**: Vendor self-declaration only (no verification)
- [ ] **Option B**: Vendor provides certification documents; manual verification by ops team
- [x] **Option C**: Only products with recognized third-party certifications (e.g., Leaping Bunny, PETA) are marked
- [ ] **Other**: [Please specify ethical marker verification process]

**Impact:** This affects user trust, legal liability, operational overhead, and brand reputation.

---

## Q12: Multi-Address Checkout Behavior

**Context:** The PRD mentions "Multiple shipping addresses support" but doesn't clarify if this means multiple addresses per user or multiple addresses per order.

**Decision Required:**

- [x] **Option A**: Users can save multiple addresses but select only one per order
- [ ] **Option B**: Users can split a single order to ship to multiple addresses (e.g., gifts)
- [ ] **Option C**: Users can save multiple addresses; one address per order; multi-address requires multiple orders
- [ ] **Other**: [Please specify multi-address behavior]

**Impact:** This affects cart/checkout complexity, order splitting logic, and shipping cost calculation.

---

## Summary

**Initial Critical Ambiguities Detected:** 12  
**Resolved:** 12  
**Remaining:** 0

### Resolution Summary

All critical ambiguities have been resolved with the following decisions:

1. **Inventory**: Near-real-time sync from external system (< 5s)
2. **Cart Persistence**: Server-side with 30-day retention for all users
3. **Payment Retry**: Immediate retry allowed with same method
4. **Order Cancellation**: User-initiated until shipment creation
5. **Wishlist Sync**: Full cross-device synchronization for authenticated users
6. **Stock Notifications**: Email alerts for back-in-stock wishlist items
7. **Guest Checkout**: Supported with post-purchase account creation encouragement
8. **Data Residency**: US-only (us-central1) for initial launch
9. **Email Automation**: Comprehensive (transactional + abandoned cart + back-in-stock)
10. **Consistency Model**: Strong for payment/order/inventory; eventual for catalog/wishlist/status
11. **Ethical Verification**: Third-party certifications required (Leaping Bunny, PETA)
12. **Multi-Address**: One address per order; gifts require separate orders

**PRD Updated:** ✅  
**Ambiguity Count:** 0  
**Ready for Roadmap Generation:** ✅

---

**Next Step:** Proceed to Phase 3 (Roadmap Generation)
