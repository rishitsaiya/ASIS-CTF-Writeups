## Tripolar
The main idea finding the flag is reversing the function approach to obtain numbers in function.


#### Step-1:
After we unzip the tar file `tripolar_fce2e4d92769431e533995a625dd8060f80a20aa.txz`, we get the `tripolar` directory.

We get `tripolar.py`, `flag.enc` & `flag.i64` in `tripolar` directory.

#### Step-2:
When we open the `tripolar.py` file, we observe that crow() function is responsible for the main encryption.

#### Step-3:

The only approach here is to find a way to get back `(x, y, z)` from `crow`. Although this function looks complicated, it can be simplified to a much nicer version.

Let `a = x + y + z + 1`, `b = x + y + 1`. With our knowledge learned in highschool, `crow` can be simplified as:

```
crow = (a**3 + 3*(b**2) + 2*b - 6*y - z - 6) // 6
```
#### Step-4:

Now because x, y, z are big numbers we can get them back with approximation:

-   `a` approximates to `cuberoot(6*crow)`
-   `b` approximates to `sqrt((6*crow - a**3) / 3)` (when x, y, z are not big enough, this value is off by one)
-   Then we can calculate the rest to get back `(x, y, z)`

#### Step-5:
The following `Crack.py` helps us to find the flag.

```py
from Crypto.Util.number import *

def crow(x, y, z):
    print('x', x)
    print('y', y)
    print('z', z)
    return (x**3 + 3*(x + 2)*y**2 + y**3 + 3*(x + y + 1)*z**2 + z**3 + 6*x**2 + (3*x**2 + 12*x + 5)*y + (3*x**2 + 6*(x + 1)*y + 3*y**2 + 6*x + 2)*z + 11*x) // 6

def root(n, k):
      low = 0
      high = n + 1
      while high > low + 1:
         mid = (low + high) >> 1
         mr = mid**k
         if mr == n:
            return (mid, 1)
         if mr < n:
            low = mid
         if mr > n:
            high = mid
      return (low, 0)

def rev_crow(cr, delta = 0):
        cr *= 6
        a = root(cr, 3)[0]
        print('a', a)
        cr = cr - a**3
        b = root(cr // 3, 2)[0] + delta
        print('b', b)
        cr = 3*b*b + 2*b - cr
        r = a - b
        cr = cr - r - 6
        q = cr // 6
        p = b - q - 1
        print('p', p)
        print('q', q)
        print('r', r)
        print('crow', crow(p, q, r))
        return p, q, r

cr = open('flag.enc', "rb").read()
cr = bytes_to_long(cr)
print('crow', cr)
pq, qr, rp = rev_crow(cr)
pqr = root(pq*qr*rp, 2)[0]
_enc = pqr // qr
_hash = pqr // pq
pk = pqr // rp
print('enc', _enc)
print('hash', _hash)
print('pk', pk)
p, q, r = rev_crow(pk, 1)
e, n = 31337, p * q * r
phi = (p - 1)*(q - 1) * (r - 1)
d = inverse(e, phi)
pt = pow(_enc, d, n)
print(long_to_bytes(pt))

```

#### Step-6:
The final flag becomes:
`ASIS{I7s__Fueter-PoLy4__c0nJ3c7UrE_iN_p4Ir1n9_FuNCT10n}`
