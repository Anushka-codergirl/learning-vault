# WooCommerce Checkout Fields Customization Guide

This document explains how to **add custom checkout fields** in
WooCommerce safely and without breaking the site.

---

## Option 1 --- Using a Plugin (No Code, Easiest)

Recommended plugins:
-   Checkout Field Editor for WooCommerce
-   Flexible Checkout Fields
-   WooCommerce Checkout Manager

### Benefits

-   No coding required
-   Fields saved automatically in orders
-   Visible in admin panel and emails
-   Easy to edit or remove later

**Best for:** Event registrations or quick setup.

---

## Option 2 --- Using Code (Lightweight, Long-term control & Clean)

Add the following code to:

    wp-content/themes/your-child-theme/functions.php

### Add Custom Checkout Fields

``` php
add_filter('woocommerce_checkout_fields', 'add_event_source_fields');
function add_event_source_fields($fields) {
    $fields['billing']['billing_event_source'] = array(
        'type'     => 'select',
        'label'    => 'How did you find out about this?',
        'required' => true,
        'class'    => array('form-row-wide'),
        'options'  => array(
            '' => 'Select an option',
            'email' => 'Email Newsletter',
            'facebook' => 'Facebook',
            'instagram' => 'Instagram',
            'whatsapp' => 'WhatsApp',
            'x' => 'X',
            'google' => 'Google Search / Website',
            'other' => 'Other'
        )
    );

    $fields['billing']['billing_referral_name'] = array(
        'type'     => 'text',
        'label'    => 'Referral Name',
        'required' => false,
        'class'    => array('form-row-wide'),
    );
    return $fields;
}
```

---

## Display Fields in Admin Order Page

``` php
add_action('woocommerce_admin_order_data_after_billing_address', 'display_event_source_admin', 10, 1);

function display_event_source_admin($order) {
    echo '<p><strong>Event Source:</strong> ' . get_post_meta($order->get_id(), '_billing_event_source', true) . '</p>';
    echo '<p><strong>Referral Name:</strong> ' . get_post_meta($order->get_id(), '_billing_referral_name', true) . '</p>';
}
```

---

## Safety Tips

Before adding code:

-   Always **backup your site**
-   Prefer a **child theme** instead of main theme
-   Test checkout after changes

---