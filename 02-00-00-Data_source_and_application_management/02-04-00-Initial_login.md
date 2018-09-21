# First login #


If you log in to Energy Log Server for the first time, you must
specify the Index to be searched. We have the option of entering the
name of your index, indicate a specific index from a given day, or
using the asterix (\*) to indicate all of them matching a specific
index pattern. Therefore, to start working with Energy Log Server
application, we log in to it (by default the user:
logserver/password:logserver).

![](/media/media/image3.png)

After logging in to the application click the button "Set up index pattern" to add new index patter in Kibana: 

![](/media/media/image3_js.png)

In the "Index pattern" field enter the name of the index or index pattern (after
confirming that the index or sets of indexes exists) and click "Next step" button.

![](/media/media/image4.png)

In the next step, from  drop down menu select the "Time filter field name", after witch individual event (events) should be sorter. By default the *timestamp* is set, which is the time of occurrence of the event, but depending of the preferences. It may also be the time of the indexing or other selected
based on the fields indicate on the event.

![](/media/media/image4_js.png)

At any time, you can add more indexes or index patters by going to the
main tab select „Management" and next select „Index Patterns".
