

```python
# import everything and define a test runner function
from importlib import reload
from helper import run_test

import ecc
import helper
```

### Exercise 1

#### 1.1. Find out which points are valid on the curve \\( y^2 = x^3 + 7: F_{223} \\)
```
(192,105), (17,56), (200,119), (1,193), (42,99)
```
#### 1.2. Write [this test](/edit/session2/ecc.py) using the results above
```
ecc.py:ECCTest:test_on_curve
```


```python
# Exercise 1.1

from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

points = ((192,105), (17,56), (200,119), (1,193), (42,99))

# iterate over points
for x_raw, y_raw in points:
    # Initialize points this way:
    # x = FieldElement(x_raw, prime)
    # y = FieldElement(y_raw, prime)
    # try initializing, RuntimeError means not on curve
    # p = Point(x, y, a, b)
```


```python
# Exercise 1.2

reload(ecc)
run_test(ecc.ECCTest('test_on_curve'))
```

### Exercise 2

#### 2.1. Find the following point additions on the curve  \\( y^2 = x^3 + 7: F_{223} \\)
```
(192,105) + (17,56), (47,71) + (117,141), (143,98) + (76,66)
```

#### 2.2. Write [this test](/edit/session2/ecc.py) using the results above
```
ecc.py:ECCTest:test_add1
```


```python
# Exercise 2.1

from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

additions = ((192, 105, 17, 56), (47, 71, 117, 141), (143, 98, 76, 66))

# iterate over the additions to be done
for x1_raw, y1_raw, x2_raw, y2_raw in additions:
    # Initialize points this way:
    # x1 = FieldElement(x1_raw, prime)
    # y1 = FieldElement(y1_raw, prime)
    # p1 = Point(x1, y1, a, b)
    # x2 = FieldElement(x2_raw, prime)
    # y2 = FieldElement(y2_raw, prime)
    # p2 = Point(x2, y2, a, b)
```


```python
# Exercise 2.2

reload(ecc)
run_test(ecc.ECCTest('test_add1'))
```

### Exercise 3

#### 3.1. Find the following scalar multiplications on the curve  \\( y^2 = x^3 + 7: F_{223} \\)
```
2*(192,105), 2*(143,98), 2*(47,71), 4*(47,71), 8*(47,71), 21*(47,71)
```

#### Hint: add the point to itself n times


```python
# Exercise 3.1

from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

multiplications = ((2, 192, 105), (2, 143, 98), (2, 47, 71), (4, 47, 71), (8, 47, 71), (21, 47, 71))

# iterate over the multiplications
for s, x_raw, y_raw in multiplications:
    # Initialize points this way:
    # x = FieldElement(x_raw, prime)
    # y = FieldElement(y_raw, prime)
    # p = Point(x, y, a, b)
    # start product at 0 (point at infinity)
    # loop over n times (n is 2, 4, 8 or 21 in the above examples)
        # add the point to the product
    # print product
```

### Exercise 4

#### 4.1. Find out what the order of the group generated by (15, 86) is on  \\( y^2 = x^3 + 7: F_{223} \\)

#### Hint: add the point to itself until you get the point at infinity

#### 4.2 Write [this test](/edit/session2/ecc.py) using the results above
```
ecc.py:ECCTest:test_rmul
```
#### 4.3 Make [this test](/edit/session2/ecc.py) pass
```
ecc.py:ECCTest:test_rmul
```


```python
# Exercise 4.1

from ecc import FieldElement, Point

prime = 223
a = FieldElement(0, prime)
b = FieldElement(7, prime)

x = FieldElement(15, prime)
y = FieldElement(86, prime)
p = Point(x, y, a, b)
inf = Point(None, None, a, b)

# start product at point
# start counter at 1
# loop until you get point at infinity (0)
    # add the point to the product
    # increment counter
# print counter
```


```python
# Exercise 4.2/4.3

reload(ecc)
run_test(ecc.ECCTest('test_rmul'))
```


```python
# Confirgming G is on the curve
p = 2**256 - 2**32 - 977
x = 0x79BE667EF9DCBBAC55A06295CE870B07029BFCDB2DCE28D959F2815B16F81798
y = 0x483ADA7726A3C4655DA4FBFC0E1108A8FD17B448A68554199C47D08FFB10D4B8
print(y**2 % p == (x**3 + 7) % p)
```


```python
# Confirming order of G is n
from ecc import G

n = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141
print(n*G)
```


```python
# Getting the public point from a secret
from ecc import G

secret = 999
point = secret*G
print(point)
```

### Exercise 5

#### 5.1. Get the public point where the scalar is the following:
```
7, 1485, 2**128, 2**240+2**31
```

#### 5.2. Make [this test](/edit/session2/ecc.py) pass
```
ecc.py:S256Test:test_pubpoint
```


```python
# Exercise 5.1

from ecc import G

secrets = (7, 1485, 2**128, 2**240+2**31)

# iterate over secrets
    # get the public point
```


```python
# Exercise 5.2

reload(ecc)
run_test(ecc.S256Test('test_pubpoint'))
```

#### Exercise 6

#### 6.1. Find the compressed and uncompressed SEC format for pub keys where the private keys are:
```
999**3, 123, 42424242
```

#### 6.2. Make [this test](/edit/session2/ecc.py) pass
```
ecc.py:S256Test:test_sec
```


```python
# Exercise 6.1

from ecc import G

secrets = (999**3, 123, 42424242)

# iterate through secrets
    # get public point
    # uncompressed - b'\x04' followed by x coord, then y coord
    # here's how you express a coordinate in bytes: some_integer.to_bytes(32, 'big')
    # compressed - b'\x02'/b'\x03' follewed by x coord. 02 if y is even, 03 otherwise
```


```python
# Exercise 6.2

reload(ecc)
run_test(ecc.S256Test('test_sec'))
```

### Exercise 7

#### 7.1. Find the mainnet and testnet addresses corresponding to the private keys:
```
compressed, 888**3
uncompressed, 321
uncompressed, 4242424242
```

#### 7.2. Make [this test](/edit/session2/ecc.py) pass
```
ecc.py:S256Test:test_address
```


```python
# Exercise 7.1
from ecc import G

from helper import double_sha256, encode_base58, hash160

components = (
    # (compressed, secret)
    (True, 888**3),
    (False, 321),
    (False, 4242424242),
)

# iterate through components
for compressed, secret in components:
    # get the public point
    # get the sec format
    # hash160 the result
    # prepend b'\x00' for mainnet b'\x6f' for testnet
        # get the double_sha256 of the prefix + h160, first 4 bytes are the checksum
        # append checksum
        # encode_base58 the whole thing
```


```python
# Exercise 7.2

reload(ecc)
run_test(ecc.S256Test('test_address'))
```

### Exercise 8

#### 8.1. Create a testnet address using your own secret key
#### (use your phone number if you can't think of anything)
#### Record this secret key for tomorrow!


```python
# Exercise 8.1
from ecc import G

# use your phone number if you can't think of anything
secret = 1800555555518005555555

# get the public point
# if you completed 7.2, just do the .address(testnet=True) method on the public point
```


```python
# Signing Example
from random import randint

from ecc import G, N

secret = 4
z = randint(0, 2**256)
k = randint(0, 2**256)
r = (k*G).x.num
s = (z+r*secret) * pow(k, N-2, N) % N
print(hex(z), hex(r), hex(s))
```


```python
# Verification Example
from ecc import G, N, S256Point

z = 0x524c14a77b666d906fbe56973becf3b3b9eac65442774473c68407e89c5659de
r = 0xc0824a3ccdf3482f1435ef1917fad4a1d5573a15f0fa18a9b81dc76a941c4a3c
s = 0x84ada30118411ef3f1777690d3dc182c289e04486375e91ba73bc48c51c59da7
point = S256Point(
0xe493dbf1c10d80f3581e4904930b1404cc6c13900ee0758474fa94abe8c4cd13, 0x51ed993ea0d455b75642e2098ea51448d967ae33bfbdfe40cfe97bdc47739922)

u = z * pow(s, N-2, N) % N
v = r * pow(s, N-2, N) % N
print((u*G + v*point).x.num == r)
```

### Exercise 9

#### 9.1. Which sigs are valid?

```
P = (887387e452b8eacc4acfde10d9aaf7f6d9a0f975aabb10d006e4da568744d06c, 
     61de6d95231cd89026e286df3b6ae4a894a3378e393e93a0f45b666329a0ae34)
z, r, s = ec208baa0fc1c19f708a9ca96fdeff3ac3f230bb4a7ba4aede4942ad003c0f60,
          ac8d1c87e51d0d441be8b3dd5b05c8795b48875dffe00b7ffcfac23010d3a395,
          68342ceff8935ededd102dd876ffd6ba72d6a427a3edb13d26eb0781cb423c4
z, r, s = 7c076ff316692a3d7eb3c3bb0f8b1488cf72e1afcd929e29307032997a838a3d,
          eff69ef2b1bd93a66ed5219add4fb51e11a840f404876325a1e8ffe0529a2c,
          c7207fee197d27c618aea621406f6bf5ef6fca38681d82b2f06fddbdce6feab6
```

#### 9.2. Make [these tests](/edit/session2/ecc.py) pass
```
ecc.py:S256Test:test_verify
ecc.py:PrivateKeyTest:test_sign
```


```python
# Exercise 9.1

from ecc import S256Point, G, N

px = 0x887387e452b8eacc4acfde10d9aaf7f6d9a0f975aabb10d006e4da568744d06c
py = 0x61de6d95231cd89026e286df3b6ae4a894a3378e393e93a0f45b666329a0ae34

signatures = (
    # (z, r, s)
    (0xec208baa0fc1c19f708a9ca96fdeff3ac3f230bb4a7ba4aede4942ad003c0f60,
     0xac8d1c87e51d0d441be8b3dd5b05c8795b48875dffe00b7ffcfac23010d3a395,
     0x68342ceff8935ededd102dd876ffd6ba72d6a427a3edb13d26eb0781cb423c4),
    (0x7c076ff316692a3d7eb3c3bb0f8b1488cf72e1afcd929e29307032997a838a3d,
     0xeff69ef2b1bd93a66ed5219add4fb51e11a840f404876325a1e8ffe0529a2c,
     0xc7207fee197d27c618aea621406f6bf5ef6fca38681d82b2f06fddbdce6feab6),
)

# initialize the public point
# use: S256Point(x-coordinate, y-coordinate)

# iterate over signatures
for z, r, s in signatures:
    # u = z / s, v = r / s
    # finally, uG+vP should have the x-coordinate equal to r
```


```python
# Exercise 9.2

reload(ecc)
run_test(ecc.S256Test('test_verify'))
run_test(ecc.PrivateKeyTest('test_sign'))
```
