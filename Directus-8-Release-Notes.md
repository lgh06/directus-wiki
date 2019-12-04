# IMPORTANT: Read through the entire list of breaking changes _before_ upgrading production projects to Directus 8. Always perform a full database backup before upgrading.

## 🔥 Breaking Changes
* 💥 The Admin App now expects to be in the API’s `/public` folder
* 💥 The `config/api.php` no longer maps to the default project (eg: `_`). All project keys are defined by their respective config file's name.
* 💥 The `app` and `settings` keys in the config file have been removed
* 💥 Password reset flow is now a `POST` request instead of a `GET`

## 🚀 Migration Guide (v7-v8)
1. Rename the config files to your desired project names (eg `/config/api.my-project.php` to `/config/my-project.php`). Note: if you'd like to keep using `_` as your project name, rename the `/config/api.php` file to `/config/_.php`). Remove `api_sample.php`, `migrations.php`, and `migrations.upgrades.php`.
2. Update `app` and `settings` in your project config file to reflect the following changes:

```
BEFORE-----------------------------------

'app' => [
    'env' => 'production',
    'timezone' => 'America/New_York'
],
'settings' => [
    'logger' => [
        'path' => __DIR__ . '/../logs',
    ],
],

-----------------------------------------
AFTER -----------------------------------

'env' => 'production',

'logger' => [
    'path' => __DIR__ . '/../logs',
],

----------------------------------------
```

## ✨ New Features
* **Platform Structure**
  * The App and API have been combined!!
  * The App config file has been removed, all public projects are automatically fetched
  * There is no longer a "default" project, all projects are equal
* **Updated Admin App Design**
  * New Login/Reset/Install pages (more info below)
  * New Customizable Module Bar (replaces User Menu)
  * New Page Info Bar (will become customizable soon)
  * New project logo size (square, 64x64)
  * Added Dark Mode, Light Mode, and Auto Mode (user configurable)
  * Added a User Popover globally
  * Updated collapsable header bar with page title and breadcrumb
  * Updated Project Switcher lets you seamlessly switch between projects
  * Cleaned up Settings, File, and User page forms
* **Updated Auth Flow & Public Pages**
  * New installation flow and design
  * Login/Reset now shows project color and/or foreground/background images
  * Stay Logged-in to multiple projects simultaneously
  * API projects can now be set to "Public", "Private", or "Disabled"
  * Added the ability to create additional projects through App
  * Added "Check Requirements" step
  * Added a SuperAdmin password for secure project installs
  * Added ability to link directly to a specific project
  * Added support for Directus Cloud (SaaS) projects
  * Proper two-factor authentication (2FA) support
  * Proper single-sign-on (SSO) support
  * Updated auth flow supports secure `httpOnly` cookies to stay logged in longer
  * App no longer requires signing out/in when switching projects, and remembers last project
  * All public pages support multilingual internationalization now
* **New and Improved Settings**
  * App allows renaming/translating the schema's Collection and Field names
  * Webhooks can now be created and managed in the App Settings
  * Override for the new Module Listing, with custom icon, name, and link (per User Role)
  * Override for the Collection Listing, show/hide, reorder, group (per User Role)
  * IP Whitelist now uses a more intuitive Tags interface
  * Telemetry can now be enabled or disabled from within the App
* **New and Improved Interfaces**
  * New full-featured WYSIWYG Interface (TinyMCE) replaces our two previous incomplete ones
  * Complete overhaul of the Repeater interface, allows nesting
  * Added a Key-Value interface
  * Redesigned and simplified the Color interface
  * Nearly all interfaces have had their styles cleaned up
* **Files & Thumbnails**
  * Brand new Asset Generator replaces our previous Thumbnailer
  * Cleaner URL/Options syntax for requesting dynamically generated assets
  * Ability to rename files on the disk
  * Middleware for private files through re-generating file URLs
  * Paves the way for other effects/transforms
  * Thumbnails can be _explicitly_ whitelisted (avoids issues with permutations)
  * Disable whitelist for generating any assets dynamically
  * Metadata can now be entered through a repeater
* **Many other bug fixes, code improvements, and small features**

## 🎱 Semantic Versioning
Now that the app is always bundled within the API, we will begin using the same version number across our repos. Gone are the days where Directus had 3 different version numbers.