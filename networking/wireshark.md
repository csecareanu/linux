## Content
* [Add new columns](#add_new_columns)
  * [How to add a new column](#add_new_column)
  * [TCP Window Size](#add_column_tcp_window_size)
  * [TCP Delta time](#add_column_tcp_delta_time)

**Any time you see something between sqare brackets `[...]` in Wireshark, it is an intepretation, it is not an actual field.**
 * [Timestamps] - _wireshark interpretation_
 * [SEQ/ACK analysis] - _wireshark interpretation_ 
 * Options: (12 bytes), No-Operation (NOP), No-Operation (NOP), Timestamps - _wireshark data_

## Add new columns <a name='add_new_columns'>
 
### How to add a new column <a name='add_new_column'>

I. You can add a new colum by opening the Wireshark settings
  * Go to: Edit -> Preferences -> Columns
  * Add a new column with data populated as for below
 
II. Or you can add a new colum by browsing a packet to the required field
  * Right click -> Apply as column
  * To change the column name -> right clic on column header -> Edit Column
 
### TCP Window Size <a name="add_column_tcp_window_size">
  * Title: Window size value
  * Typee: Custom 
  * Fields: tcp.window_size_value  (or you can use the field tcp.window_size if you want to work with the calculated scaling window value)

 ### TCP Delta time <a name="add_column_tcp_delta_time">
 
 TCP Delta Time measures how much time elapsed between the prior and current packet in the conversation, for each connection, separately.
 
  * Title: TCP Delta
  * Typee: Custom 
  * Fields: tcp.time_delta

This filed coresponds to wireshark interpretation:
```
[Timestamps]
   [Time since first frame in this TCP stream: 0.191943000 seconds]\
   [Time since previous frame in this TCP stream: 0.000023000 seconds] -> Time dela from previous frame in this TCP stream (tcp.time_delta)
```
