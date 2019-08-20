# kcp协议
https://github.com/skywind3000/kcp/issues/137
## 协议格式
```
+-------+-----+------+------+-----+-----+-----+----+-----+------+----------------------------+
| NONCE | CRC | CONV |  CMD | FRG | WND | TS  | SN | UNA | LEN  |           DATA             |
+-------+-----+------+------+-----+-----+-----+----+-----+------+----------------------------+
|  16   | 4   |   4  |   1  |  1  |  2  |  4  |  4 |  4  |  4   |          variable          |
+-------+-----+------+------+-----+-----+-----+----+-----+------+---+---+---+---+------------+
|                            SESSION                            |VER|CMD|LEN|SID|   DATA     |
+---------------------------------------------------------------+---+---+---+---+------------+
|                             44                                | 1 | 1 | 2 | 4 |  variable  |
+---------------------------------------------------------------+---+---+---+---+------------+
```
### 说明
- NONCE：16-bytes cryptographically secure random number, nonce changes for every packet
- CRC：checksum of data using the IEEE polynomial
   NONCE + CRC = overall crypto header size
- CONV：random num for each flow
- CMD：
    * CMD_PUSH 81：push data
    * CMD_ACK  82：ack
    * CMD_WASK 83：window probe (ask)
    * CMD_WINS 84：window size (tell)
- FRG：fragment count
- WND：window size
- TS：timestamp
- SN：serial number
- UNA：un-acknowledged serial number
- LEN：data length
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
