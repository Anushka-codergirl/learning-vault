# Finding and Editing `functions.php` in WordPress

This guide explains how to locate and safely edit the
**`functions.php`** file to add custom code.

---

## Method 1 --- WordPress Dashboard (Easiest)

1.  Go to **WordPress Admin → Appearance → Theme File Editor**
2.  On the right panel, find: **Theme Functions (`functions.php`)**
3.  Click the file to open and edit.

⚠️ **Important** - Always edit the **child theme's** `functions.php` if
available.
- Editing the main theme file may lose changes after theme updates.

---

## Method 2 --- cPanel / Hosting File Manager

1.  Log in to **cPanel → File Manager**
2.  Navigate to:

```{=html}
<!-- -->
```
    public_html/wp-content/themes/your-theme-name/functions.php

3.  Right-click the file → **Edit**

---

## Method 3 --- FTP (Advanced)

Use an FTP client like **FileZilla** and open:

    wp-content/themes/your-theme-name/functions.php

Download → edit → upload back.

---

## Safety Tips (Very Important)

Before making any changes:
-   Create a **backup** of `functions.php`
-   A small code error can **break the entire website**
-   Best practice:
    -   Use a **child theme**, OR
    -   Create a **small custom plugin** instead of editing theme files

---