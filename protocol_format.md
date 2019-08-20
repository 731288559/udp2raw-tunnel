# udp2raw-tunnel
https://github.com/wangyu-/udp2raw-tunnel
## 协议格式
```
+----------+----------+------------+---------------+------+-----------+---------------+
| conv_num |   my_id  | oppsite_id |     n_seq     | type | my_roller |      DATA     | 
+----------+----------+------------+---------------+------+-----------+---------------+
|     4    |     4    |      4     |       8       |   1  |      1    |   variable    | 
+----------+----------+------------+---------------+------+-----------+---------------+

```
### 说明
- conv_num：16-bytes cryptographically secure random number, nonce changes for every packet
- my_id：checksum of data using the IEEE polynomial
   NONCE + CRC = overall crypto header size
- oppsite_id：random num for each flow
- n_seq：
- type：fragment count
- my_roller：window size

- DATA
    * VER：1
    * CMD：
        * cmdSYN：stream open
        * cmdFIN：stream close, a.k.a EOF mark
        * cmdPSH：data push
        * cmdNOP：no operation
    * LEN：data length
    * SID：client start from 1, sever start from 0, for each flow step by step 2
    * DATA：udp data
