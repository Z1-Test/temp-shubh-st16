# Epic: Wishlist Management

**Bounded Context:** Wishlist (within Cart context)  
**Status:** Draft  
**Owner:** Product Team

---

## Overview

The Wishlist Management epic encompasses capabilities that allow authenticated users to save products for future consideration, receive notifications when products become available, and sync wishlists across devices. This epic enables users to plan future purchases and stay informed about product availability.

---

## Strategic Importance

The Wishlist context drives repeat engagement and purchases by:
- Enabling users to curate product collections for future purchase
- Notifying users of availability changes to drive conversion
- Providing insights into product interest and demand
- Building long-term customer relationships

---

## Bounded Context Scope

The Wishlist context is responsible for:
- Wishlist creation and management (add, remove items)
- Cross-device wishlist synchronization
- Back-in-stock notification enrollment
- Email notification delivery for availability changes

The Wishlist context does **not** handle:
- Wishlist sharing or social features (out of scope)
- Product recommendations (deferred)
- Wishlist-to-cart bulk operations (future enhancement)

---

## Features in this Epic

1. **Wishlist Management** - Allow authenticated users to save products for later with cross-device sync
2. **Back-in-Stock Notifications** - Notify users via email when wishlist products become available

---

## Key Dependencies

**Upstream Dependencies:**
- User Authentication (wishlists require authenticated users)
- Product Catalog Display (to display product details in wishlist)
- Inventory Synchronization (to detect when products come back in stock)

**Downstream Consumers:**
- Email Notification System (sends back-in-stock notifications)
- Analytics (tracks wishlist behavior)

---

## Success Criteria

- Authenticated users can reliably save and access wishlists across devices
- Back-in-stock notifications are timely and accurate
- Wishlist operations are fast and responsive
- Users receive clear confirmation of wishlist actions

---

## Business Value

- Increases customer lifetime value through repeat engagement
- Drives conversion of previously interested but unavailable products
- Provides demand signals for inventory planning
- Builds customer loyalty through personalized notifications
- Supports mobile shopping with seamless sync

---

## Related Documents

- PRD: `/docs/product/PRD.md`
- Roadmap: `/docs/product/roadmap.md`
- Feature Specifications:
  - `/docs/features/wishlist/wishlist-management.md`
  - `/docs/features/wishlist/back-in-stock-notifications.md`
