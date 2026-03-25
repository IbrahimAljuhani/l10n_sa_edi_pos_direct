# Changelog — l10n_sa_edi_pos_direct

---

## [18.0.1.4.0] — 2026-03-25

### Contributors
- Ibrahim Aljuhani

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

## [18.0.1.3.0] — 2025-08-29

### Changed
- 🔧 **Enhanced QR Code Integration:** Improved override of compute_sa_qr_code method to properly use l10n_sa_pos functions
- 🚀 **Code Optimization:** Removed redundant QR generation methods and streamlined date formatting

---

## [18.0.1.2.0] — 2025-08-22

### Fixed
- 🔧 **Fixed Arabic Character Encoding:** Resolved btoa() InvalidCharacterError when using Arabic language interface
- 🛡️ **Improved Unicode Support:** Enhanced base64 encoding for ZATCA compliance with Arabic text

---

## [18.0.1.1.0] — 2025-08-20

### Added
- ✅ **Added ZATCA Refund Features:** Interactive refund reason popup with 6 predefined codes and full ZATCA compliance (BR-KSA-17, BR-KSA-F-04)
