# GTT-Based Product Update and CSV Export

The SQL Script uses `CTS_ORDER_ITEMS_GTT` as a temporary staging table for order items belonging to a selected product. It increments each staged row’s quantity, then merges the changes into `CTS_ORDER_ITEMS` using `(ORDER_ID, LINE_ITEM_ID)` as the matching key.

After committing the update, the script exports the matching rows from the main table as a CSV file to OCI Object Storage using `DBMS_CLOUD.EXPORT_DATA`. An APEX page process can retrieve that file with `DBMS_CLOUD.GET_OBJECT` and deliver it to the browser using `APEX_HTTP.DOWNLOAD`.

**Operational note:** Run the GTT workflow together in one script execution. The update is committed before the export, so an export failure does not roll back the table changes. Check the data before rerunning to avoid incrementing quantities twice.
