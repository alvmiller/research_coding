# C : Number System Conversion

- https://www.geeksforgeeks.org/c/number-system-conversion-in-c/
- https://labex.io/tutorials/c-convert-numbers-between-bases-in-c-435169
- https://numeral-systems.com/converter/
- https://www.geeksforgeeks.org/c/c-binary-to-decimal/
- https://labex.io/tutorials/cpp-how-to-convert-between-number-systems-427284
- https://docs.vultr.com/clang/examples/convert-binary-number-to-octal-and-vice-versa
- https://www.learncpp.com/cpp-tutorial/numeral-systems-decimal-binary-hexadecimal-and-octal/#google_vignette
- https://www.cs.trincoll.edu/~ram/cpsc110/inclass/conversions.html

![pic1](https://www.geeksforgeeks.org/wp-content/uploads/binary2decimal.png "pic1")

```c
#include <stdio.h>

int binaryToDecimal(int n) {
    int dec = 0;

    // Initializing base value to 1, i.e 2^0
    int base = 1;
    
    // Extracting each digits of binary number
    // and adding corresponding exponent of 2
    while (n) {
        int last_digit = n % 10;
        n = n / 10;

        // Multiplying the last digit with the base value
        // and adding it to the decimal value
        dec += last_digit * base;

        // Updating the base value by multiplying it by 2
        base = base * 2;
    }

    return dec;
}

int main() {
    int num = 10101001;
    printf("%d", binaryToDecimal(num));

    return 0;
}
```

```c
#include <stdio.h>
#include <string.h>

int binaryToDecimal(const char* binary) {
    int dec = 0;
    
    // Get length of binary string
    int length = strlen(binary);
    
    // Initializing base value to 1, i.e 2^0
    int base = 1;
    
    // Process from right to left (least significant to
    // most significant bit)
    for (int i = length - 1; i >= 0; i--) {
        
        // If current bit is '1'
        if (binary[i] == '1') {
            dec += base;
        }
        
        // Update base for next position (multiply by 2)
        base = base * 2;
    }
    
    return dec;
}

int main() {
    const char* binary = "10101001";
    printf("%d\n", binaryToDecimal(binary));
    
    return 0;
}
```






































