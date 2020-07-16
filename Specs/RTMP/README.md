# RTMP Spec
Describe RTMP(Real-Time Message Protocol) Spec

## RTMP Packet Header
기본 RTMP Packet header의 값은 12bytes 이나 Basic Header의 값을 1, 2 byte 정도 늘릴 수 있다.
<br>
<img src="../../Images/RTMP_PacketHeader.png" width="50%" height="50%">
<br>
첫 fmt와 stream id는 기본 header(basic header), 그 후 stream id(cont) 2 byte는 늘리고 싶을때 늘릴 수 있는 사이즈(extended basic header).

## RTMP Packet Example
<br>
<img src="../../Images/RTMP_PacketHeaderExample.png" width="100%" height="100%">
<br>