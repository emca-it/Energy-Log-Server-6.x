Object access permissions (Objects permissions)
-----------------------------------------------

In the User Manager tab we can parameterize access to the newly 
created role as well as existing roles. In this tab we can indicate 
to which object in the application the role has access.

Example:

In the Role List tab we have a role called **sys2**, it refers
to all index patterns beginning with syslog\* and the methods get,
post, delete, put and head are assigned.

![](/media/media/image56_js.png)

When we go to the Object permission tab, we have the option to choose
the sys2 role in the drop-down list choose a role:

![](/media/media/image57_js.png)

After selecting, we can see that we already have access to the objects:
two index patterns syslog2\* and Energy Log Server-\* and on dashboard Windows Events. 
There are also appropriate read or updates permissions.

![](/media/media/image58_js.png)

From the list we have the opportunity to choose another object that we
can add to the role. We have the ability to quickly find this object
in the search engine (Find) and narrowing the object class in
the drop-down field "Select object type". The object type are associated
with saved previously documents in the sections Dashboard, Index pattern, 
Search and Visualization. 
By buttons ![](/media/media/image59.png) we have the ability to add or remove or
object, and Save button to save the selection.
