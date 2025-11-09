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

# ONION Protocol
- https://community.torproject.org/onion-services/overview/
- https://www.geeksforgeeks.org/computer-networks/onion-routing/
- https://nordvpn.com/blog/onion-routing/?srsltid=AfmBOorbJEMCNqWmaJ7C48qfkz8jUGffeWurrTVqy__2-siZPTxU-QUt
- https://www.voltage.cloud/blog/what-is-onion-routing-how-does-it-work

![pic1](https://en.wikipedia.org/wiki/Onion_routing#/media/File:Onion_diagram.svg "pic1")
![pic1](https://www.researchgate.net/publication/354458981/figure/fig2/AS:1065941441990657@1631151652957/Tor-hidden-onion-service-protocol-Source-19.ppm "pic1")

> Onion routing is a technique for anonymous communication over a computer network.
> In an onion network, messages are encapsulated in layers of encryption, analogous to layers of an onion.
>
> Understanding Onion routing concept an example
> Now suppose you are browsing the internet using Tor(the onion router)
> which is a special browser that lets you use the onion routers.
> You want to access YouTube but you live in China and since YouTube is banned in China
> you don't want your government to know that you are visiting YouTube so you decide to use Tor.
> Your computer needs to contact a particular server to get the homepage of YouTube
> but it doesn't directly contact that server. It does that through 3 nodes/servers/routers
> (these servers are maintained all over the world by volunteers)
> before that server so that no one can trace back your conversation with that server.
> To make this example simple I am using 3 nodes but a real Tor network can have hundreds of nodes in between. 

![pic1](https://media.geeksforgeeks.org/wp-content/uploads/Onion-Routing-Page-1.png "pic1")

> Onion Routing Circuit(made using lucid chart)
> 1. The client with access to all the encryption keys i.e key 1, key 2 & key 3 encrypts the message(get request)
>    thrice wrapping it under 3 layers like an onion which have to be peeled one at a time.
> 2. This triple encrypted message is then sent to the first server i.e. Node 1(Input Node).
> 3. Node 1 only has the address of Node 2 and Key 1. So it decrypts the message using Key 1
>    and realizes that it doesn't make any sense since it still has 2 layers of encryption so it passes it on to Node 2
> 4. Node 2 has Key 2 and the addresses of the input & exit nodes.
>    So it decrypts the message using Key 2 realizes that it's still encrypted and passes it onto the exit node
> 5. Node 3 (exit node) peels off the last layer of encryption and finds a GET request
>    for youtube.com and passes it onto the destination server
> 6. The server processes the request and serves up the desired webpage as a response.
> 7. The response passes through the same nodes in the reverse direction where each node puts on
>    a layer of encryption using their specific key
> 9. It finally reaches the client in the form of a triple encrypted response which can be decrypted
>     since the client has access to all the keys




































