# Scheduler Module #

Energy Log Server has a built-in task schedule. In this module, we can
define a command or a list of commands whose execution we instruct the
application in the form of tasks. We can determine the time and
frequency of tasks. Tasks can contain a simple syntax, but they can
also be associated with modules, e.g. with Intelligence module.

To go to the Scheduler window, select the tile icon from the main menu
bar and then go to the „Scheduler" icon (To go back, go to the
„Search" icon)

![](/media/media/image38_js5.png)

The page with three tabs will be displayed: Creating new tasks in the
„Create Scheduler Job", managing tasks in the „Job List" and checking
the status of tasks in „Jobs Status"

In the window for creating new tasks we have a form consisting of
fields:

- **Name** - in which we enter the name of the task
- **Cron Pattern** - a field in which in cron notation we define the time
   and frequency of the task
- **Command** - we give the syntax of the command that will be executed
   in this task. These can be simple system commands, but also
   complex commands related to the Intelligence module. In the task
   management window, we can activate /deactivate, delete and update
   the task by clicking on the selected icon for a given task
   ![](/media/media/image63.png)

In the task status windows you can check the current status of the
task: if it activated, when it started and when it ended, how long it
took. This window is not editable and indicates historical data.
