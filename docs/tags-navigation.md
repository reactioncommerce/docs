<!-- # Tags and Navigation -->

## The basics

Mailchimp Open Commerce offers flexible organization of products with tags and a hierarchical navigation system. One or more tags can be applied to products (but not to variants). In turn, tags can be linked to navigation items, which can be nested within each other. These navigation items appear when browsing the shop. The navigation hierarchy is also reflected in the breadcrumbs shown on each product detail page.

## Creating tags

To create a tag, log in to the Open Commerce admin dashboard, click **Tags** in the left sidebar, and click **Create a tag**. Then enter the tag name, display title, and slug. If you want the tag to appear immediately, check "Tag is enabled in storefront".

Tag names and slugs must be unique within a shop. When creating tags designed for a multi-level hierarchy, it is important to take this into account. The process for creating a tag is the same regardless of what level of the navigation hierarchy it will occupy. However, when creating child tags it is a good practice to include the parent tag in the name and slug to prevent conflicts. For example, a store selling language textbooks could have tags with the slugs `french`, `french-beginner`, `french-advanced`, `mandarin`, `mandarin-beginner`, `mandarin-advanced`, but it could not have two tags with the slug `beginner`, one for each language.

> Note: Multiple tags can have the same display title. For example, `french-beginner` and `mandarin-beginner` could both have the display title "Beginner".

## Assigning tags

After creating tags, you can use them to indicate what categories a product belongs to. For more information on assigning tags to products, see [Creating and Organizing Products](creating-and-organizing-products).

If you've created a hierarchical tag system and you want a product to appear at all levels of the hierarchy, you will need to assign it multiple tags, such as tagging a beginner French textbook with both `french-beginner` and `french`.

## Creating navigation items

Tags themselves are not organized hierarchically. To create a hierarchical organization and have it display in the shop, you must create navigation items, each of which should correspond to a single tag.

To create a navigation item, click **Navigation** in the left sidebar and then click **Add navigation item**. In the dialog, fill out the following fields:

- **Display Name** – The name that will appear in the shop navigation.
- **URL** – Enter `/tag/YOUR_TAG_SLUG`. The leading slash is required.
- **This URL is relative** – Check this box.

Once you have created your navigation items, drag and drop them from the lefthand list of all items into the righthand list of active items. You can organize the active navigation items as you want by dragging an item vertically to change its position in the list or horizontally to change its level of nesting.

When added to the hierarchy, navigation items are hidden by default. To show a navigation item, click the pencil icon next to its name in the hierarchy (on the right) and check the **Show in storefront** box.
