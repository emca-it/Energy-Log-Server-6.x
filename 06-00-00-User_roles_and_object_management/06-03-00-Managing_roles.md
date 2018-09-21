# Create, modify and delete a role (Create Role), (Role List)  #

In the Create Role tab we can define a new role with permissions that 
we assign to a pattern or several index patterns.

![](/media/media/image54_js.png)

In example, we use the syslog2\* index pattern. We give this name
in the Paths field. We can provide one or more index patterns, their
names should be separated by a comma. In the next Methods field, we
select one or many methods that will be assigned to the role. Available
methods:

- PUT - sends data to the server
- POST - sends a request to the server for a change
- DELETE - deletes the index / document
- GET - gets information about the index /document
- HEAD - is used to check if the index /document exists

In the role field, enter the unique name of the role. We confirm addition
of a new role with the Submit button. To see if a new role has been added, 
go to the net Role List tab.

![](/media/media/image55_js.png)

As we can see, the new role has been added to the list. With the
Delete button we have the option of deleting it, while under the
Update button we have a drop-down menu thanks to which we can add or
remove an index pattern and add or remove a method. When we want to
confirm the changes, we choose the Submit button. Pressing the Update
button again will close the menu.

Fresh installation of the application have sewn solid roles which
granting user special rights:

- admin - this role gives unlimited permissions to administer / manage
the application
- alert - a role for users who want to see the Alert module
- kibana - a role for users who want to see the application GUI
- Intelligence - a role for users who are to see the Intelligence module