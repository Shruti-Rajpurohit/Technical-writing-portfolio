# WooCommerce Guide: Setting Up and Managing Your Online Store
## Table of Contents
- [Introduction](#introduction)
- [Before You Begin](#before-you-begin)
- [Installing WooCommerce](#installing-woocommerce)
    * [To Install WooCommerce](#to-install-woocommerce)
- [Initial Store Setup](#initial-store-setup)
- [Adding Your First Product](#adding-your-first-product)
    * [To add a product](#to-add-a-product)
    * [Types of products](#types-of-products)
- [Setting Up Payments](#setting-up-payments)
    * [To configure payments](#to-configure-payments)
- [Setting Up Shipping](#setting-up-shipping)
    * [To configure shipping](#to-configure-shipping)
- [Managing Orders](#managing-orders)
    * [To view and manage orders](#to-view-and-manage-orders)
- [Customizing Your Store](#customizing-your-store)
- [Testing Your Store](#testing-your-store)
    * [Checklist](#checklist)
- [Troubleshooting](#troubleshooting)
- [Tips for a Better Store](#tips-for-a-better-store)
- [What's Next?](#whats-next)
- [Related Guides](#related-guides)
## Introduction
If you want to sell any product or service through your WordPress site, WooCommerce is one of the most widely used tools to do that.
This guide will walk you through:
* Setting up your store
* Adding products
* Configuring payments and shipping
* Managing orders

You do not require any prior eCommerce experience to get started. Take it step by step, and your store can be set up in no time.

---
## Before You Begin
Before setting up WooCommerce, make sure:
* Your WordPress site is up and running
* You have access to a plan that supports plugins
* You have basic information handy regarding:
    * Store name
    * Product information
    * Payment method (bank account, UPI, etc.)
> **Note:** WooCommerce requires a Business plan or higher on WordPress.com. It is not available on free or lower-tier plans.
---
## Installing WooCommerce
### To install WooCommerce:
1. Go to **Plugins → Add New**
2. Search for **WooCommerce**
3. Click **Install**
4. Click **Activate**
Once activated, a setup wizard will guide you through initial configuration.
> **Beginner Mistake:** Installing WooCommerce but skipping the setup wizard. This often leads to missing store pages or incomplete settings.
### Common Issues:
**Issue: WooCommerce not installing**
* Check your internet connection
* Refresh the Plugins page and try again
* Log out and log back in

**Issue: Plugin installs but activation fails** 
* Try deactivating other plugins and retry
* Reload dashboard and attempt activation again

**Issue: WooCommerce menu not appearing after activation**
* Refresh dashboard
* Check if plugin is actually activated under Plugins

---
## Initial Store Setup
WooCommerce will ask you for:
* Store location
* Currency
* Type of products
* Business details

Proper completion of these helps in getting prices, taxes, and shipping rates correct.
> **Common Mistake:** Selecting the wrong currency or country. This can cause issues with payments later.
### Common Issues:
**Issue: Setup wizard did not appear**
* Go to WooCommerce → Home to relaunch setup
* If not visible, check WooCommerce settings manually

**Issue: Incorrect store details already saved** 
* Go to WooCommerce → Settings → General
* Update currency, location, and units
---
## Adding Your First Product
### To add a product:
1. Go to **Products → Add New**
2. Enter product name and description
3. Add product price under **Product Data**
4. Upload product images
5. Select product category
6. Click **Publish**
---
### Types of products:
* Simple product
* Variable product (size, color, etc.)
* Digital or downloadable product
* Grouped product (a collection of related products)

> **Common Mistakes:**
> * Forgetting to set a price, which prevents the product from being purchased
> * Not adding images, which reduces trust and engagement
### Common Issues:
**Issue: Product not visible on site**
* Check if product is Published, not Draft
* Ensure visibility is set to Public
* Confirm it is not set as “Hidden” in catalog visibility

**Issue: Product shows but cannot be purchased** 
* Make sure price is set
* Check if product is marked “Out of stock”
* Verify inventory settings

**Issue: Images not appearing** 
* Re-upload image
* Check if image is properly attached to product

---
## Setting Up Payments
WooCommerce supports multiple payment methods such as:
* Bank transfer
* Cash on delivery
* Online payments (via gateways)

Popular payment gateways include PayPal, Stripe, and Razorpay (recommended for India-based stores).

### To configure payments:
1. Go to **WooCommerce → Settings → Payments**
2. Enable the payment methods you want
3. Complete setup for each method

> **Common Mistakes:**
> * Enabling a payment method but not completing its setup
> * Not testing the payment flow before going live

### Common Issues:
**Issue: Payment option not visible at checkout**
* Ensure payment method is enabled
* Complete all required setup fields
* Check if method supports your region

**Issue: Payment fails during checkout**
* Verify account credentials
* Try test transaction
* Check if payment gateway requires additional verification

**Issue: Only one payment option showing** 
* Enable multiple payment methods in settings

---
## Setting Up Shipping
### To configure shipping:
1. Go to **WooCommerce → Settings → Shipping**
2. Add a shipping zone (for example, India)
3. Add shipping methods:
    * Flat rate
    * Free shipping
    * Local pickup
> **Common Mistakes:** 
> * Not setting any shipping method, which prevents checkout
> * Forgetting to match shipping zones with customer locations
### Common Issues:
**Issue: No shipping options visible at checkout**
* Check shipping zones
* Confirm customer location falls under a defined zone
* Ensure at least one shipping method is added

**Issue: Incorrect shipping cost**
* Review flat rate settings
* Check if multiple shipping rules are conflicting

**Issue: Free shipping not working**
* Confirm minimum order condition is met
* Check if free shipping is enabled in correct zone

---
## Managing Orders
### To view and manage orders:
1. Go to **WooCommerce → Orders**
2. Click on any order to view details
3. Update order status:
    * Processing
    * Completed
    * Cancelled
> **Common Mistake:** Not updating order status, which can confuse customers about their purchase.

You can add an order note visible to customers when updating status. It is useful for sharing tracking information or estimated delivery dates.

### Common Issues:
**Issue: Orders not appearing**
* Refresh Orders page
* Check filters such as date or status

**Issue: Customer not receiving order confirmation**
* Ask customer to check spam folder
* Verify email settings in WooCommerce

**Issue: Order stuck in “Processing”**
* Confirm payment was completed
* Update status manually if needed

---
## Customizing Your Store
You can customize your store by:
* Changing themes
* Editing product pages
* Adjusting layout using blocks
> **Common Mistake:** Switching themes without checking WooCommerce compatibility, which can break layouts.

### Common Issues:
**Issue: Layout looks broken after theme change**
* Switch back to previous theme to confirm cause
* Check if theme supports WooCommerce

**Issue: Product pages look different than expected**
* Some themes override default layouts
* Adjust settings in theme customizer

---
## Testing Your Store
Before making your store public, test everything.
> **Tip:** Before testing, enable test mode in your payment gateway settings. This allows you to simulate purchases without real transactions.
### Checklist:
* Add a product to cart
* Complete checkout
* Test payment method
* Verify order confirmation
> **Common Mistake:** Launching the store without testing the full purchase flow.
### Common Issues:
**Issue: Checkout not completing**
* Check payment setup
* Disable plugins temporarily to test conflict

**Issue: Cart not updating**
* Refresh page
* Try incognito mode

**Issue: Errors during test purchase**
* Note exact error message
* Retry after clearing cache

---
## Troubleshooting
If the issue is unclear:
* Undo your most recent change
* Disable plugins one by one
* Switch to a default theme temporarily
* Test after each step

This helps isolate the root cause instead of guessing. You can always reach out to support.

---
## Tips for a Better Store
* Use clear product images
* Write clear, honest product descriptions
* Keep navigation easy
* Avoid adding too many plugins
* Enable customer reviews to build trust
* Set up tax settings correctly for your region under **WooCommerce → Settings → Tax**
* Use product categories to help   customers find items easily
* Keep your WooCommerce plugin updated to avoid security issues
---
## What's Next?
Now that your store is set up, you can:
* Add more products
* Improve design and branding
* Explore marketing tools
* Track performance and sales
---
## Related Guides
* [Getting Started with WordPress](https://github.com/Shruti-Rajpurohit/Technical-writing-portfolio/blob/main/docs/wordpress-doc-suite/wordpress-onboarding-guide.md)
* [WordPress Help Guide](https://github.com/Shruti-Rajpurohit/Technical-writing-portfolio/blob/main/docs/wordpress-doc-suite/WordPress%20Help%20Guide.md)
* [WordPress Troubleshooting Guide](https://github.com/Shruti-Rajpurohit/Technical-writing-portfolio/blob/main/docs/wordpress-doc-suite/wordpress-troubleshooting-guide.md)
