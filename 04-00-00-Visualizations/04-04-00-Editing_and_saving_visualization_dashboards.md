# Edit visualization and saving #

## Edititing ##
Editing a saved visualization enables you to directly modify the object 
definition. You can change the object title, add a description, and modify
the JSON that defines the object properties.
After selecting the index and the method of data presentation, you can enter
the editing mode. This will open a new window with empty
visualization.

![](/media/media/image29_js.png)

At the very top there is a bar of queries that cat be edited
throughout the creation of the visualization. It work in the same way
as in the "Discover" tab, which means searching the raw data, but
instead of the data being displayed, the visualization will be edited.
The following example will be based on the „Area chart". The
visualization modification panel on the left is divided into three tabs:
„Data", "Metric & Axes" and „Panel Settings".

In the „Data" tab, you can modify the elements responsible for which data
and how should be presented. In this tab there are two sectors:
"metrics", in which we set what data should be displayed, and
„buckets" in which we specify how they should be presented. 

Select the Metrics & Axes tab to change the way each individual metric is shown 
on the chart. The data series are styled in the Metrics section, while the axes 
are styled in the X and Y axis sections.

In the „Panel Settings" tab, there are settings relating mainly to visual
aesthetics. Each type of visualization has separate options. 

To create the first graph in the char modification panel, in the „Data" tab we
add X-Axis in the "buckets" sections. In „Aggregation" choose „Histogram", 
in „Field" should automatically be located "timestamp" and "interval": 
"Auto" (if not, this is how we set it). Click on the
icon on the panel. Now our first graph should show up.

Some of the options for „Area Chart" are:

   **Smooth Lines** - is used to smooth the graph line.

![](/media/media/image30.png)

-   **Current time marker** -- places a vertical line on the graph that
    determines the current time.

-   **Set Y-Axis Extents** -- allows you to set minimum and maximum
    values for the Y axis, which increases the readability of the
    graphs. This is useful, if we know that the data will never be
    less then (the minimum value), or to indicate the goals the
    company (maximum value).

-  **Show Tooltip** -- option for displaying the information window
    under the mouse cursor, after pointing to the point on the graph.
   
   ![](/media/media/image31.png)
   
## Saving ##
To save the visualization, click on the "Save" button under on the query bar:
![](/media/media/image16.png)
give it a name and click the button
![](/media/media/image17.png). 

## Load ##
To load the visualization, go to the "Management Object"
 -> "Saved Object" -> "Visualizations" select it from the list. From this place, 
we can also go into advanced editing mode. To view of the visualization 
use ![](/media/media/image17_js.png) button.