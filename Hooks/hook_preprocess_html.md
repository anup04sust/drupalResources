# Drupal HTML Hook Variables Documentation

This document describes the variables available in the Drupal `html` theme hook preprocess layer.

## Hook Context

The following variables were captured from a `dd($variables)` call inside a Drupal HTML preprocess hook:

```php
function mymodule_preprocess_html(array &$variables) {
  dd($variables);
}
```

## Available Variables

### Root Variables

| Variable              | Type                               | Description                                                       |
| --------------------- | ---------------------------------- | ----------------------------------------------------------------- |
| `html`                | array                              | HTML-related rendering information.                               |
| `theme_hook_original` | string                             | Original theme hook name. Usually `html`.                         |
| `attributes`          | array                              | Attributes applied to the `<body>` element.                       |
| `title_attributes`    | array                              | Attributes for the page title element.                            |
| `content_attributes`  | array                              | Attributes for the main content wrapper.                          |
| `title_prefix`        | array                              | Render array displayed before the page title.                     |
| `title_suffix`        | array                              | Render array displayed after the page title.                      |
| `db_is_active`        | boolean                            | Indicates whether the database connection is active.              |
| `is_admin`            | boolean                            | Indicates whether the current page is an administrative page.     |
| `logged_in`           | boolean                            | Indicates whether the current user is authenticated.              |
| `user`                | `Drupal\Core\Session\AccountProxy` | Current user account object.                                      |
| `directory`           | string                             | Active theme directory path.                                      |
| `page`                | array                              | Main page render array containing page regions and content.       |
| `page_top`            | array                              | Render array displayed at the top of the page.                    |
| `page_bottom`         | mixed                              | Render array displayed at the bottom of the page.                 |
| `html_attributes`     | `Drupal\Core\Template\Attribute`   | Attributes applied to the `<html>` element.                       |
| `root_path`           | mixed                              | Current root path information.                                    |
| `head_title`          | array                              | Components used to build the page title.                          |
| `placeholder_token`   | string                             | Internal Drupal placeholder token used for rendering.             |
| `#attached`           | array                              | Attached assets such as CSS, JavaScript, libraries, and metadata. |

---

## Variable Details

### `theme_hook_original`

```php
"theme_hook_original" => "html"
```

Identifies the original theme hook being processed.

---

### `logged_in`

```php
"logged_in" => false
```

Returns:

- `true` → User is authenticated.
- `false` → Anonymous visitor.

Example:

```php
if ($variables['logged_in']) {
  // Show member-only content.
}
```

---

### `is_admin`

```php
"is_admin" => false
```

Indicates whether the current page belongs to the administrative interface.

Example:

```php
if ($variables['is_admin']) {
  $variables['attributes']['class'][] = 'admin-page';
}
```

---

### `user`

```php
"user" => Drupal\Core\Session\AccountProxy
```

Provides access to the currently logged-in user.

Example:

```php
$uid = $variables['user']->id();
$name = $variables['user']->getDisplayName();
```

---

### `directory`

```php
"directory" => "themes/custom/iom_ap_rdh"
```

Path to the active theme directory.

Example:

```twig
<img src="/{{ directory }}/images/logo.svg" alt="Logo">
```

---

### `html_attributes`

```php
"html_attributes" => Drupal\Core\Template\Attribute
```

Attributes applied to the `<html>` tag.

Example output:

```html
<html lang="en" dir="ltr"></html>
```

Add custom attributes:

```php
$variables['html_attributes']['data-theme'] = 'custom';
```

---

### `head_title`

```php
"head_title" => [...]
```

Contains the page title components.

Example:

```php
$title = implode(' | ', $variables['head_title']);
```

---

### `page`

```php
"page" => [...]
```

Contains all rendered page regions such as:

- Header
- Navigation
- Content
- Sidebar
- Footer

Example:

```php
$content = $variables['page']['content'];
```

---

### `page_top`

```php
"page_top" => [...]
```

Render array displayed immediately after the opening `<body>` tag.

---

### `page_bottom`

```php
"page_bottom" => null
```

Render array displayed before the closing `</body>` tag.

Useful for:

- JavaScript attachments
- Analytics snippets
- Dynamic page elements

---

### `#attached`

```php
"#attached" => [...]
```

Contains attached assets and metadata.

Common structure:

```php
$variables['#attached']['library'][] = 'mytheme/global';
```

Example libraries:

```php
mytheme/global
core/drupal
core/jquery
```

---

### `placeholder_token`

```php
"placeholder_token" => "..."
```

Drupal internal token used during render caching and placeholder replacement.

Generally not modified directly.

---

## Example Preprocess Usage

```php
/**
 * Implements hook_preprocess_html().
 */
function iom_ap_rdh_preprocess_html(array &$variables) {

  // Add class for anonymous users.
  if (!$variables['logged_in']) {
    $variables['attributes']['class'][] = 'anonymous-user';
  }

  // Add theme directory.
  $variables['theme_directory'] = $variables['directory'];

  // Detect admin pages.
  if ($variables['is_admin']) {
    $variables['attributes']['class'][] = 'admin-page';
  }
}
```

---

## Related Drupal Documentation

Relevant APIs:

- `hook_preprocess_html()`
- `Drupal\Core\Template\Attribute`
- `Drupal\Core\Session\AccountProxy`
- Theme preprocessing system
- Render arrays
- Asset libraries

---

## Captured Environment

| Property        | Value                      |
| --------------- | -------------------------- |
| Theme Hook      | `html`                     |
| Theme Directory | `themes/custom/iom_ap_rdh` |
| Database Active | `true`                     |
| Admin Page      | `false`                    |
| Logged In       | `false`                    |
