# l10n_sa_edi_pos_direct

**Enhanced Saudi Arabia ZATCA Integration for Point of Sale**

An optimized replacement for the standard `l10n_sa_edi_pos` module, providing direct ZATCA integration with significant performance improvements and enhanced functionality.

## Overview / نظرة عامة

An optimized replacement for the standard `l10n_sa_edi_pos` module, providing direct ZATCA integration with significant performance improvements and enhanced functionality designed specifically for high-volume retail environments in Saudi Arabia.

موديول محسن بديل عن `l10n_sa_edi_pos` الافتراضي، يوفر تكامل مباشر مع هيئة الزكاة والضريبة مع تحسينات كبيرة في الأداء ووظائف محسنة

## Module Comparison / مقارنة الموديولات

| Feature / الخاصية | l10n_sa_edi_pos (Standard / الافتراضي) | l10n_sa_edi_pos_direct (Enhanced / المحسن) |
|---------|---------------------------|-----------------------------------|
| **Customer Data Requirement / متطلبات بيانات العميل** | ❌ Requires full customer data for every POS transaction<br/>يتطلب بيانات العميل كاملة مع كل عملية | ✅ Cash customer support (no customer data needed)<br/>دعم العميل النقدي (لا يتطلب بيانات العميل) |
| **PDF Generation / إنشاء ملف PDF** | ❌ Creates A4 PDF invoice for every POS order<br/>ينشئ فاتورة PDF مع كل طلب | ✅ No PDF generation - POS receipts only<br/>لا ينشئ PDF - إيصالات نقاط البيع فقط |
| **Database Load / حمل قاعدة البيانات** | ❌ Need to connect database with every transaction<br/>يتطلب الاتصال بقاعدة البيانات مع كل عملية | ✅ Minimal database impact - frontend processing<br/>تأثير قليل على قاعدة البيانات - معالجة على واجهة نقاط البيع فقط |
| **QR Code Compliance / امتثال رمز QR** | ❌ POS Receipts QR Code with Basic Phase 1 (only 5 fields)<br/>رمز QR بالمرحلة الأولى الأساسية (5 حقول فقط) | ✅ POS Receipts QR Code with Full Phase 2 (9 fields + digital signatures)<br/>رمز QR بالمرحلة الثانية كاملة (9 حقول + توقيعات رقمية) |
| **ZATCA Synchronization / مزامنة هيئة الزكاة** | ❌ Immediate sync required (not required for simplified)<br/>مزامنة فورية مطلوبة (غير مطلوبة للمبسطة) | ✅ Compliant 24-hour async reporting<br/>تقارير غير متزامنة خلال 24 ساعة حسب اللوائح |
| **Record Duplication / تكرار السجلات** | ❌ POS Order + Account Invoice (double records)<br/>طلب نقاط البيع + فاتورة محاسبية (سجلات مضاعفة) | ✅ Single POS record - no duplication<br/>سجل نقاط البيع واحد - لا تكرار |



## Installation & Configuration

1. **Remove Standard Module**: Uninstall the existing `l10n_sa_edi_pos` module from your system to avoid conflicts
2. **Install Enhanced Module**: Install `l10n_sa_edi_pos_direct` through the Apps menu or via command line
3. **Verify Configuration**: Ensure your ZATCA certificate is properly configured in the invoice journal settings
4. **Enable Direct Mode**: Navigate to POS Configuration and enable "ZATCA Direct Mode" for your point of sale
5. **Test Transactions**: Perform test transactions to verify QR code generation and ZATCA submission workflow

## Changelog

### Version 18.0.1.4.2 — 2026-08-02
**Contributor:** Ibrahim Aljuhani

**Fixed:**
- `order_receipt.xml` was extending a non-existent template (`l10n_sa_pos.ReceiptHeader`) — corrected to `point_of_sale.ReceiptHeader`, so the header-QR removal for direct-mode orders now applies at the QWeb level instead of relying entirely on CSS/JS force-hiding
- `.zatca-qr-img` used a fixed 450px size with `min-width`/`min-height`, which overflowed Odoo's actual print container (266px in `@media print`) — could clip the QR on real printed receipts. Changed to `width: 100%; max-width: 300px;` so it scales down to fit the real paper width
- Removed duplicate `image-rendering` declaration (`pixelated` was dead code, overridden by `crisp-edges`)

**Removed:**
- Redundant `MutationObserver` in `pos_store.js` that force-hid the header QR via inline styles — no longer needed now that the QWeb-level fix above correctly omits the header QR for direct-mode orders

---

### Version 18.0.1.4.1 — 2026-08-02
**Contributor:** Ibrahim Aljuhani

**Fixed:**
- BR-KSA-F-04 violation: negative-price promo/discount lines are now emitted as document-level `AllowanceCharge` (reason code 95) instead of negative `InvoiceLine` amounts
- Line `unit_price` now derived from `price_subtotal` instead of raw `price_unit`, keeping `InvoiceLine`/`Price` consistent with line-level POS discounts (BR-KSA-EN16931-11)
- Duplicate ZATCA submission handling for HTTP 409 ("Invoice was already Reported successfully earlier"): detection now matches the actual ZATCA reporting API response shape (`validationResults.errorMessages[].message`) instead of a top-level `error` key and enum-style strings that don't exist in the real response
- `batch_submit_pending_zatca` now commits and persists `error` status/message per order instead of losing state on exception

**Removed:**
- Unused `queue_job` dependency from `__manifest__.py` — background processing is done via `ir.cron`, not `queue_job`; will be reintroduced once actually wired up and tested

---

### Version 18.0.1.4.0 — 2026-03-25
**Contributor:** Ibrahim Aljuhani

**Added:**
- QR Code moved to bottom of receipt (after order number and date)
- QR Code size set to 450×450px for better readability
- Hide "Powered by Odoo" from POS receipt
- Hide Odoo logo from Customer Display (sidebar and main area)

**Changed:**
- Improved `MutationObserver` — auto-disconnects after first successful execution for better performance
- Cleaned `zatca_pos.css` — removed unused dead CSS classes
- Updated `__manifest__.py` — replaced CSS wildcard with explicit file list
- Removed empty `customer_display.xml`

**Fixed:**
- QR Code duplication when ZATCA Direct Mode is enabled

---

### Version 18.0.1.3.0
🔧 **Enhanced QR Code Integration:** Improved override of compute_sa_qr_code method to properly use l10n_sa_pos functions  
🚀 **Code Optimization:** Removed redundant QR generation methods and streamlined date formatting

### Version 18.0.1.2.0
🔧 **Fixed Arabic Character Encoding:** Resolved btoa() InvalidCharacterError when using Arabic language interface

### Version 18.0.1.1.0
✅ **Added ZATCA Refund Features:** Interactive refund reason popup with 6 predefined codes and full ZATCA compliance (BR-KSA-17, BR-KSA-F-04)

## Support

This module is designed for **Saudi Arabian businesses** requiring **high-performance ZATCA compliance** in retail environments.

---

**Author**: EasyERPS, AMR Hawsawi, Ibrahim Aljuhani  
**License**: LGPL-3  
**Website**: https://easyerps.com
