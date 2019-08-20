# udp2raw-tunnel
https://github.com/wangyu-/udp2raw-tunnel
## 协议格式
```
except handshakes and 3 packets after handshakes ? 
FIN unknown yet
+-----------+------------+---------------+------+-----------+---------------+
|   my_id   | oppsite_id |     n_seq     | type | my_roller |      DATA     |
+-----------+------------+---------------+------+-----------+---------------+
|     4     |     4      |       8       |   1  |     1     |   variable    |
+-----------+------------+---------------+------+-----------+---------------+
if type = 0x64:
+-----------+------------+---------------+------+-----------+------------+---------------+
|   my_id   | oppsite_id |     n_seq     | type | my_roller |  conv_num  |      DATA     |
+-----------+------------+---------------+------+-----------+------------+---------------+
|     4     |     4      |       8       |   1  |     1     |     4      |   variable    |
+-----------+------------+---------------+------+-----------+------------+---------------+

```
### 说明
- my_id：       my unique id in a conversation
- oppsite_id：  opposite unique id in a conversation
- n_seq：       seq_id
- type：
   * 0x68(h):   heart beat
   * 0x64(d):   data
 
- my_roller：   increase on a successful recv, only for record recv times?
- conv_num      conversation id
- DATA
    * DATA：udp data
    
  
   handshakes and 3 packets after handshakes dont like this
   (heartbeat)
   f6f359bb 53327d4e 0d9be642a1ce909d 68 08
   53327d4e f6f359bb 0559318fc8ae3f67 68 09
   ...
   53327d4e f6f359bb 0559318fc8ae3f62 68 04 
   f6f359bb 53327d4e 0d9be642a1ce9099 68 04
   53327d4e f6f359bb 0559318fc8ae3f63 68 05
   f6f359bb 53327d4e 0d9be642a1ce909a 68 05
   
   
   53327d4e f6f359bb 0559318fc8ae3f68 68 0d
   f6f359bb 53327d4e 0d9be642a1ce90a2 68 0a
   53327d4e f6f359bb 0559318fc8ae3f69 68 0e
   ...
   (data)
   f6f359bb 53327d4e 0d9be642a1ce909e 64 09 6bf6ca54 /73656e64206d657373616765 (data)
   f6f359bb 53327d4e 0d9be642a1ce909f 64 09 6bf6ca54 /61616161
