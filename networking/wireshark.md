## Content
* [Add new columns](#add_new_columns)
  * [TCP Window Size](#add_comulum_tcp_window_size)

## Add new columns <a name='add_new_columns'>
* Go to: Edit -> Preferences -> Columns
* Add a new column with data populated as for below
 
### TCP Window Size <a name="add_comulum_tcp_window_size">
  * Title: Window size value
  * Typee: Custom 
  * Fields: tcp.window_size_value  (or you can use the field tcp.window_size if you want to work with the calculated scaling window value)

 ### Delta time
 
 TCP Delta Time measures how much time elapsed between the prior and current packet in the conversation.
 
  * Title: TCP Delta Time
  * Typee: Custom 
  * Fields: tcp.time_delta
