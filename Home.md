# Getting Started

## Requirements
Directus is a forward-looking framework, not built to support legacy systems. Its requirements are:

* Apache HTTP Server
* mod_rewrite
* PHP 5.5+
* curl
* gd
* MySQL 5.2+
* pdo_mysql

Optionally, you can install the following for added functionality:
* Imagick – _Adds thumbnail support for TIFF, PSD, and PDF files_
* mcrypt

_If your server is not compatible with the above requirements please contact your host to upgrade._

## Installation
* Check that your server meets the requirements above
* Download and unzip the Directus package from here
* Create a database and MySQL user (with access and modify privileges) for Directus on your web server
* Upload the files to a specific folder or the root of your web-server
* Run the Directus installation script by accessing the URL where you uploaded the files.

**Once you have completed the install, make sure to delete install folder for security purposes.**

## Upgrading
1. **Backup your database**
2. Download and unzip the Directus package from here
3. Overwrite your old version with the new files, preserving: (todo)

## Branding
Directus is minimally stylized and completely unbranded. This means that the system can be completely branded to match the client/project with little effort. This is especially important when building a platform with user visible components such as a POS. As an administrator you can tailor the system's label and logo (170x100) within `Settings > Global Settings`.

# Using Directus
## Tables
This straightforward page gives a listing of all tables your user has access to see. For ease of access, databases with fewer tables will list all tables in the sidebar. Tables that have been bookmarked (see below) will appear bold in this list.

## Item Listing
### Filtering
Choosing a field from the “Add Filter” dropdown will provide an input for quick and easy searching of larger datasets. Each filter is considered an “AND”, limiting results using one of several inputs: date-chooser, checkbox, dropdown, auto-complete, and the default text-box. Clicking the “×” will remove a given filter.

### Active, Inactive, & Delete
For tables that contain an “active” column, when selecting items from the listing page you will have the option to change their active status:
* Active – These items are considered production-ready.
* Draft – Items in draft mode are grayed out visually and this column is saved with a value of 2. Applications should exclude these items from production viewing.
* Delete – This effectively removes items from Directus, though no longer visible, due to the soft delete policy items are maintained within the database with an active value of 0. Applications should exclude these items from production viewing.

### Changing Visible Columns
By default Directus shows the first few columns of data for any given table. Users can then adjust which columns they see by hovering over the gray table header and selecting the gear on the right.

### Sorting
As is the case with more tabular data, clicking column headers will allow for the sorting of data by that field. Clicking a column again will reverse the sort direction – clicking a third time will remove sorting reverting to the default: date created. This is automatically saved on a per user basis and is persistent until changed.

### Snapshots
As with most datasets, sometimes there are multiple specific views for any given dataset. To save your current layout, simply click the snapshot button from within the toolbar and it will be added to the sidebar for your user. Snapshots save the following parameters: filters, sort column and direction, visible columns, and visible active states

### Reordering
For tables that contain a “sort” column, items will automatically show a handle on the left side of the listing page and become re-orderable through drag-and-drop.

### Bulk Edit
Selecting more than one item with the left checkboxes will give a toolbar option for “Batch Editing”. Clicking into this mode will take you to a blank edit page where edits can be made. On this page, clicking “edit” for a particular field indicates that Directus should save that value for all chosen items.

### Bookmarking
For databases that contain a large number of tables, instead of the sidebar listing all tables, Directus can be setup to only show user bookmarked tables (individual user preference). Tables are bookmarked by clicking the star at the top of the item listing page.

### Footer
A footer can be turned on for individual tables allowing additional columnar functionality. For instance, numeric columns will provide options for: Average, Min, Max, and Sum

## Create & Edit
### Core and Custom UIs
The edit page is comprised of different inputs based on the architecture, datatypes, and defaults of the table. While there are a finite number of Core UIs to cover most cases – it is easy to extend or create new inputs for specific needs. There is no limit to the styling or complexity of custom UIs.
### Saving
* Save
* Save and Stay
* Save as Copy
* Save and New

### Revision History
Every change made within Directus is tracked to give comprehensive accountability. The user, edit time, changes/delta, and complete item backup are saved with each edit.

### Item Comments

## Activity
This page shows a chronological overview of all Item Revision History (see previous page)  within the system.

## Files
This page provides a listing of all files uploaded or linked to the Directus system. Most base information is extracted from the files when added, however any number of additional fields can be added to the database table for additional meta information.

## Messages
There are two types of messages within Directus:
### Item Comments
Every item within the system can contain comments attached within the Item History for associating specific feedback.

### User Messages
Similar to email, users can send messages to other Directus users. An option exists to have all Messages forwarded to your email address.

## Users and Groups
### Access Control List (ACL)
* **View** – Ability to view your items (created by you)
* **BigView** – Ability to create the items of other users
* **Add** – Ability to add new items
* **Edit** – Ability to edit or change your items, including status changes
* **BigEdit** – Same as edit but for other user’s items
* **Delete** – Ability to delete your items – the soft-delete policy enforces that items are removed from Directus but not deleted from the database
* **BigDelete** – Same as delete but for other user’s items
* **Alter** – Permission for making schema level changes such as field order

### IP Whitelist
If turned on for a user-group, those users will only be able to connect to Directus when accessing over specific IP addresses. This is useful for ensuring a group of users can’t access the system from personal, public, or insecure networks/computers.

## Settings
### Global
Personalize this instance of Directus to fit your organization’s needs. Change colors, add a company logo, set the auto logout time, etc

### Files and Media
Here you can adjust the size and quality of your automatic thumbnails, decide where to save uploaded files, and much more.

### Tables and Inputs
Within Tables and Inputs you can make broad and granular adjustments to how Directus handles your specific data, visualizations, and inputs.

### User Permissions
Manage user groups which control access to your data. Setup table and field level permission for each group.

# Extending Directus
## Custom Extensions
By design, Directus keeps things simple. Only key features utilized by more than 80% of users make it into the core software. Anything not covered under the base suite is possible within extensions. Extensions take advantage of the system’s authentication, ACL, and data connections but force absolutely no restrictions. These sandboxed areas can be used to create custom interfaces, data-visualizations, interaction points, dashboards, point-of-sales, or anything else required by your project.
**[todo]**

## Custom Inputs
Similar to extensions, if you need an input more specific to your application than provided by core Directus, you can duplicate, tweak, or create your own. Need a custom room-mapping interface or a proprietary client input? Just create a quick custom UI and it will immediately be available for use.
**[todo]**

## Custom Listings
**[todo]**

## Custom Storage Adapters
Beyond local storage, Directus comes with the ability to connect to a number of popular file storage systems such as CDNs. As with most things within Directus, if you need something more specific the tools exist to easily implement a custom adapter.
**[todo]**

## Custom API Endpoints
If you’re using Directus' API endpoints (possibly feeding a mobile app or providing a SASS interface) you can always write custom endpoints to compliment the core data/schema/user access.
**[todo]**