The access control layer was built in response to the need to maintain the privacy and integrity of certain pieces of data within the database, as managed by Directus. This functionality allows Directus administrators to prevent certain groups from reading and writing to specific fields, and from performing certain operations on given tables.

### The `directus_privileges` table
Each row on this table is associated with a table (`table_name`) and a Directus user group (`group_id`). The `permissions` column itemizes which operations the group may perform on this table.

The `read_field_blacklist` functions by omitting the given column from the database schema which the API yields, such that the front-end app has no awareness of this column. The `write_field_blacklist` prevents that user group from writing to this column.

_The `permissions` column can contain the following, comma-separated values:_
* `add` - The ability to insert into the table.
* `alter` - The ability to modify the schema.
* `bigdelete` - The ability to delete records not owned by the current user. (This applies to all records if the table has no “magic owner column” configured.)
* `bigedit` - The ability to edit records not owned by the current user. (This applies to all records if the table has no “magic owner column” configured.)
* `delete` - The ability to delete records owned by the current user. (Requires that the table has a “magic owner column” configured.)
* `edit` - The ability to edit records owned by the current user. (Requires that the table has a “magic owner column” configured.)
* `view` - The ability to view the table. (Without this permission, the table will be completely omitted from that user’s schema.)

### The magic owner column
In order to make effective use of the `bigdelete`, `bigedit`, `delete`, and `edit` permissions, you need to define the “magic owner column” for the table in question.

This is the column which is defined as containing the ID of the Directus User who “owns” a given record.

Define the “magic owner column” by populating the `directus_tables . magic_owner_column` value for the desired table.

_This field by default should contain these values for the following core Directus tables:_
* `directus_users => id`
* `directus_media => user`
* `directus_bookmarks => user`
* `directus_preferences => user`

### The `directus_tab_privileges` table
This table allows you to create a tab blacklist per user group.