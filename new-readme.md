# CLUB420 - AGE GATE SYSTEM

## Overview

The age gate on Club420.com is a **custom-built compliance layer** implemented directly into the Divi WordPress theme. This modal-based gate ensures that **only users aged 21+** can access any content on the WordPress site — including homepages, WooCommerce sections, and all landing pages — before any store, product, or Blaze ECOM content is loaded.

Unlike Blaze’s built-in age gate, which only appears on `/menu/*` proxy URLs, this custom implementation ensures full-site compliance and seamless integration into the user experience.

---

## 🔥 Why We Built Our Own Age Gate

Blaze ECOM provides its own age verification modal — but only within its embedded menu environment:

* ✅ Only appears on Blaze `/menu/` URLs
* ❌ Does NOT protect homepage, landing pages, WooCommerce carousels, etc.
* ⚠️ Creates a compliance gap for underage users landing on `/`, `/fstreet`, `/highway80`, or other content

👉 To solve this, we **custom-coded our own age gate**, injected via Divi’s Body JS, that appears immediately upon site entry — before any store logic or redirection is possible.

This prevents ANY cannabis product content or imagery from loading **until verification is confirmed**.

---

## Behavior Flow

```text
User lands on ANY Club420.com page (including subdomains)
└──> Custom age gate modal appears (unless localStorage is already set)
     └──> User selects "21+" or "Under 21"
         ├── If "Under 21" → redirect to Google (denial path)
         └── If "21+" → store key in localStorage → allow access to page
```

Once accepted, the gate won’t reappear unless the user clears their localStorage manually.

---

## Code Breakdown

### 🧠 JS Logic (from `body-js.json`)

```js
if (!localStorage.getItem("club420-age-gate")) {
  const modalHTML = `
    <div class="club420-age-modal">
      <div class="modal-inner">
        <h2>Are you 21 or older?</h2>
        <div class="modal-buttons">
          <button class="age-yes">Yes</button>
          <button class="age-no">No</button>
        </div>
      </div>
    </div>`;

  document.body.insertAdjacentHTML("beforeend", modalHTML);
  document.body.classList.add("club420-age-body-lock");

  document.querySelector(".age-yes").addEventListener("click", () => {
    localStorage.setItem("club420-age-gate", "true");
    document.querySelector(".club420-age-modal").remove();
    document.body.classList.remove("club420-age-body-lock");
  });

  document.querySelector(".age-no").addEventListener("click", () => {
    window.location.href = "https://www.google.com";
  });
}
```

### 🎨 CSS Styling (from `divi-css-code.json`)

```css
.club420-age-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #000;
  color: #fff;
  z-index: 999999;
  display: flex;
  align-items: center;
  justify-content: center;
  animation: fadeIn 0.25s ease-out;
}

.club420-age-body-lock {
  overflow: hidden !important;
  height: 100vh !important;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
```

---

## File Map

| Component     | File                 | Notes                                                   |
| ------------- | -------------------- | ------------------------------------------------------- |
| JS Logic      | `body-js.json`       | Handles modal injection, button logic, and localStorage |
| CSS Styling   | `divi-css-code.json` | Controls visual overlay and body lock                   |
| DOM Injection | JS Only              | No static HTML file – modal created dynamically         |

---

## Integration Dependencies

* ✅ Runs **before all other JS logic**, including store selector and Woo filters
* ✅ Ensures compliance before showing ANY store/product content
* ✅ Functions even on mobile or in private browsing
* ❌ Not a plugin — uses inline JS and Divi custom code modules only
* ❌ No cookies used (localStorage only)

---

## Developer Notes

* To **manually test the modal**, run:

```js
localStorage.removeItem("club420-age-gate")
```

* Be sure your JS is inside a `DOMContentLoaded` block to prevent modal from firing too early
* You may add test flags like `console.log("Age gate modal triggered")` for debugging

---

## Compliance Summary

* Prevents underage users from accessing:

  * WordPress homepage
  * Divi pages
  * WooCommerce carousels
  * All embedded Blaze links
* Required by state cannabis compliance laws for all digital content

---

## Works With:

* ✅ WooCommerce carousels (see `03-woocommerce-filters.md`)
* ✅ Store selector logic (see `02-store-selector.md`)
* ✅ All Blaze menu links (see `04-button-injector.md`)

---

## Conclusion

The CLUB420 age gate is a **critical compliance layer** that guarantees no product or cannabis-related content is visible until a user confirms they are 21+. It is lightweight, hardcoded, and fully decoupled from Blaze — giving Club420 full control across the entire WordPress multisite structure.
