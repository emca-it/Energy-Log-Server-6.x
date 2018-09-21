# Screen content for the Trend algorithm #

![](/media/media/image68_js.png)

Description of controls:

- **feature to analyze from search** - analyzed feature (dictated)
- **multiply by field** - enable multiplication of algorithms after
    unique values of the feature indicated here. Multiplication allows
    you to run the AI rule one for e.g. all servers. The value "none" in
    this field means no multiplication.
- **multiply by values** - if a trait is indicated in the „multiply by
    field", then unique values of this trait will appear in this field.
    Multiplications will be made for the selected values. If at least
    one of value is not selected, the „Run" buttons will be inactive.\`
- **time frame** - feature aggregation method (1 minute, 5 minute, 15
    minute, 30 minute, hourly, weekly, monthly, 6 months, 12 months)
- **max probes** - how many samples back will be taken into account for
    analysis. A single sample is an aggregated data according to the
    aggregation method.
- **value type** - which values to take into account when aggregating for
    a given time frame (e.g. maximum from time frame, minimum, average)
- **max predictions** - how many estimates we make for ahead (we take
    time frame)
- **data limit** - limits the amount of date downloaded from the source.
    It speeds up processing but reduces its quality
- **start date** - you can set a date earlier than the current date in
    order to verify how the selected algorithm would work on historical
    data
- **Scheduler** - a tag if the rule should be run according to the plan
    for the scheduler. If selected, additional fields will appear;

![](/media/media/image67.png)

- **Prediction cycle** - plan definition for the scheduler, i.e. the
    cycle in which the prediction rule is run (e.g. once a day, every
    hour, once a week). In the field, enter the command that complies
    with the cron standard. Enable -- whether to immediately launch
    the scheduler plan or save only the definition
- **Role** - only users with the roles selected here and the
    administrator will be able to run the defend AI rules The selected
    „time frame" also affects the prediction period. If we choose
    "time frame = monthly", we will be able to predict a one month
    ahead from the moment of prediction (according to the "prediction
    cycle" value)
- **Threshold** - default values -1 (do not search). Specifies the
    algorithm what level of exceeding the value of the feature „feature
    to analyze from cheese" is to look for. The parameter currently used
    only by the "Trend" algorithm.
