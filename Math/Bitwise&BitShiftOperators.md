# Bitwise and Bit Shift Operators

1. The unary bitwise complement operator "~" inverts a bit pattern.

2. The signed left shift operator "<<" shifts a bit pattern to the left.
```Java
//n*2
int mulTwo(int n) {
    return n << 1;
}

//n*(2^m)，multiply m powers of 2
int mulTwoPower(int n,int m) {
    return n << m;
}
```

3. The signed right shift operator ">>" shifts a bit pattern to the right.
```Java
//n/2, not apply to negative odd number
int divTwo(int n) {
    return n >> 1;
}

//n/(2^m)，divide m powers of 2, not apply to negative odd number
int divTwoPower(int n,int m) {
    return n >> m;
}
```

4. The unsigned right shift operator ">>>" shifts a zero into the leftmost position.

5. The bitwise & operator performs a bitwise AND operation.
```Java
//Odd or even validation
boolean isOddNumber(int n){
    return (n & 1) == 1;
}
```

6. The bitwise ^ operator performs a bitwise exclusive OR operation.
```Java
// Symmetric encryption: we will have the original number,
// after we use it to do exclusive OR two times against a same number
public String encode(String source, int key) {
    byte[] b = source.getBytes("UTF-8");
    for (int i=0, size=b.length; i<size; i++) {
        for (byte keyBytes0 : keyBytes) {
            b[i] = (byte) (b[i]^key);
        }
    }
    return new String(b);
}

public void swapNumber(int a, int b) {
    a = a ^ b;
    b = a ^ b; //(a^b)^b == a
    a = a ^ b; //(a^b)^a == b
}
```

7. The bitwise | operator performs a bitwise inclusive OR operation.