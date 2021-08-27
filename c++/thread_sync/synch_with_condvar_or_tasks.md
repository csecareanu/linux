In case you are using **promise and future** to synchronize threads, they have a lot in common with **condition variables**. But most of the time, task are the better choice.


|Criteria | Condition Variables | Tasks
|---|---|---|
|Multiple synchronization possible| Yes | No |
|Critical region | Yes | No |
|Exception handling in receiver | No | Yes |
|Spurious wakeup| Yes | No |
|Lost wakeup | Yes | No|

