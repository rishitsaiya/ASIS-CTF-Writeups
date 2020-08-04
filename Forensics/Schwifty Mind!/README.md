## Schwifty Mind!
The main idea finding the flag is using simple TCP packets in Wireshark.


#### Step-1:
After we unzip the tar file `Schwifty_Mind_366b5cd0e1356857b96ab561790346885d3d252b.txz`, we get the `Schwifty_Mind` directory.

We get `Schwifty_Mind.pcap` in `Schwifty_Mind` directory.

#### Step-2:
When we open the `pcap` file in Wireshark application and check the TCP packets, there are 248 stream.

This can be checked by checking in Statistics -> Conversations

#### Step-3:

If you check the Port A->B packets, they are of number 15-16 and other range is 44-45.

<b>15-16</b> is taken as low i.e. <b>0</b>
<b>44-45</b> is taken as low i.e. <b>1</b>

Convert this range into binary format of 0s and 1s.
#### Step-4:
We get the following binary code as follows:

```
01000001010100110100100101010011011110110110110000110011011101000011010101011111011001110011001101110100010111110
10100110110001101101000011101100111011000110001011001100011011101111001010111110011010001100111011000010011000101
0011100010000101111101
```

#### Step-5:
Converting this to ASCII fetches us the flag.

#### Step-6:
The final flag becomes:
`ASIS{l3t5_g3t_Schvv1f7y_4ga1N!}`