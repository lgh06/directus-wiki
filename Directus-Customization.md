### Customizing the Navigation Sidebar
By default, Directus displays all available tables alphabetically in the navigation sidebar under the "Tables" group header. To customize this you can build a tailored JSON string and save it within **`directus_tab_privileges->nav_override`** for the desired user group.

```
{
    "Office": {
        "Staff": {
            "path": "/tables/staff"
        },
        "Locations": {
            "path": "/tables/locations"
        }
    },
    "Portfolio": {
        "Projects/Work": {
            "path": "/tables/projects"
        },
        "Clients (Brands)": {
            "path": "/tables/clients"
        }
    }
}
```