# API Endpoints

**Base API URL: `{{DIRECTUS_ROOT}}/api/1/`**


Resource | Description
:---------|:----------------
GET  privileges/:groupId/  |  Returns JSON object of the privileges for a given group
GET tables/:table/rows/  |  Returns a collection of table entries viewable by authenticated user for given table
GET tables/:table/rows/:id  |  return JSON object of a given table entry
GET activity/  |  Return a collection of latest activity
GET tables/:table/columns/  |  Returns a collection of the column details for a given table.
GET tables/:table/columns/:column/  |  Return details for a given column in a given table.
GET groups/  |  Return a collection of directus Groups
GET groups/:id  |  Return the group information for a given group
GET preferences/:table  |  Return the preferences for a given table
GET bookmarks/  |  Return the bookmarks for currently authenticated user
GET bookmarks/:id  |  Return bookmark details for a specified bookmark (Must belong to authenticated user)
GET tables/  |  Return collection of all tables registered with Directus
GET messages/rows  |  Return collection of messages for authenticated user
GET messages/rows/:id  |  Return Message details for a given message



# Global Parameters

Parameter  |  Example  |  Description
:-----------|:-----------|:-----------------------
currentPage  |  *0 , 1 , 2*  |  Current Page for Paginated Results
perPage  |  *100 , 200 , 300*  |  Results to show per page
sort  |  *id , title , date_uploaded*  |  Column to sort results upon
sort_order  |  *ASC, DESC*  |  Order direction for sorting
active  |  *1 , 1,2 , 2*  |  Filter by a CSV of status values **for tables with a status column** such as `active`
columns_visible  |  *title , columns_visible=title&columns_visibile=first_name*  |  Name of column to show in results, can be chained


# Error Responses

### Authorization:

**Not Authenticated:** You must be logged in to access the API.

View Permissions not Strictly Enforced yet (ex. table listing) for privileges, just authentication level

### Parse
Any Non-JSON Response: Error

@TODO: Additional error responses


AUTHENTICATION

Two levels of Authentication

Authentication Privileges follow the users group that the key was generated from

API Key: Single Consumer Key that is generated for each user. Passed as parameter with every API resource that uses this level
- Used with all GET Resources

OAuth: Consumer Key and Secret is generated for each User. This is used in regular OAuth flow to aquire OAuth Tokens to use with all requests on this level.
- Used with all POST/PUT Resources

GET  privileges/:groupId/

Returns JSON object of the privileges for a given group

Example Request
GET
http://directus.dev/api/1/privileges/0
[
  {
    "id": "23",
    "table_name": "countries",
    "permissions": "add,edit,bigedit,delete,bigdelete,alter,view,bigview",
    "group_id": "0",
    "read_field_blacklist": null,
    "write_field_blacklist": null,
    "unlisted": null
  },
  {
    "id": "1",
    "table_name": "directus_activity",
    "permissions": "add,edit,bigedit,delete,bigdelete,alter,view,bigview",
    "group_id": "0",
    "read_field_blacklist": null,
    "write_field_blacklist": null,
    "unlisted": null
  },
  {
    "id": "18",
    "table_name": "directus_bookmarks",
    "permissions": "add,edit,bigedit,delete,bigdelete,alter,view,bigview",
    "group_id": "0",
    "read_field_blacklist": null,
    "write_field_blacklist": null,
    "unlisted": null
  }
]



GET  tables/:table/rows/

Returns a collection of table entries viewable by authenticated user for given table

ids - Comma delimited list of ids to return
	ex. 1,2,3,5,6,7,8
Example Request
GET
http://directus.dev/api/1/tables/directus_users/rows
{
  "active": 1,
  "inactive": 0,
  "trash": 0,
  "total": 1,
  "rows": [
    {
      "id": 3,
      "active": 1,
      "first_name": "Jason",
      "last_name": "Smith",
      "email": "jason@rngr.com",
      "password": "asfafspojd92en1oi2n31b412ubb1n",
      "salt": "5329e597d9afa",
      "position": "",
      "email_messages": 1,
      "last_login": null,
      "last_access": null,
      "last_page": "",
      "ip": "",
      "group": {
        	"id": 0,
        	"name": "Administrator",
        	"description": null,
        	"restrict_to_ip_whitelist": 0
      },
      "avatar": null,
      "location": "",
      "phone": "",
      "address": "",
      "city": "",
      "state": "",
      "zip": ""
    }
]

SECURITY
All API calls pass through ACL.

Passwords:

Passwords use CRYPT_BLOWFISH
Generates Random salts when password Hashed, Encodes the Hash type, salt and stretching iteration count into “Hash Encoding String”. On Comparison, it reads said string to retrieve necessary information.
Uses /dev/urandom for randomness

Uses 8 iteration password stretching
128 bit encryption key

Database Security:

Uses Prepared Statements for all database interaction.
Uses ZendDb module for out-of-the-box security.

Timing Attacks:
Account Email probing is theoretically possible.
Can dummy salt so consistent response time if desired feature

Password Reset:
When New Password Requested, Nullifies existing password and sends new unique password token to email associated with account

XSS:
Currently, XSS within Directus is possible. This is a design decision to give full control to the client install. Any Malicious Data needs to be weeded out in the Web Application/Data Entry point, else Directus can become compromised.

Session Hijacking:
Currently, nothing is done to minimize potential attack vector from session Hijacking. Possible advancement could be to validate session with request metadata to provide minimal security.

Extensions/Custom Endpoints
Easy process to add custom features/Pages into Directus.

Create files to add new sandboxed endpoints using Slim routes.