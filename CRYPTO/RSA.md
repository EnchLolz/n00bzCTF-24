# RSA

Category: CRYPTO

Points: xxx

Solves: xxx

>The cryptography category is incomplete without RSA. So here is a simple RSA challenge. Have fun!

### Solution

```
e = 3
n = 135112325288715136727832177735512070625083219670480717841817583343851445454356579794543601926517886432778754079508684454122465776544049537510760149616899986522216930847357907483054348419798542025184280105958211364798924985051999921354369017984140216806642244876998054533895072842602131552047667500910960834243
c = 13037717184940851534440408074902031173938827302834506159512256813794613267487160058287930781080450199371859916605839773796744179698270340378901298046506802163106509143441799583051647999737073025726173300915916758770511497524353491642840238968166849681827669150543335788616727518429916536945395813
```

Classic Small `e` RSA:

```py
from Crypto.Util.number import long_to_bytes
from sympy import integer_nthroot

while True:
    cbrt, perfect = integer_nthroot(c,3)
    if perfect:
        print(cbrt)
        print(long_to_bytes(cbrt))
        break
    c+=n

# output
235360648501923597413504426673122110620436456645077837051697081536135487875222175025616363200782717
b'n00bz{crypt0_1s_1nc0mpl3t3_w1th0ut_rs4!!}'
```

Or just upload to dcode :skull:

### Flag

```n00bz{crypt0_1s_1nc0mpl3t3_w1th0ut_rs4!!}```