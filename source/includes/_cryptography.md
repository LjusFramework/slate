
# Cryptography

## Password Hashing
> To hash passwords, using the following approach:

```cpp
#include "ljus/hash/Hash.h"

string plain = "passw0rd";
string hashed = Hash::make(plain);

bool match = Hash::check(password, hashed); // true
bool needs_rehash = Hash::needs_rehash(hashed); //false
```

> Let's look at how to rehash passwords:

```cpp
string plain = "passw0rd";
string old_hashed = //Get the old hash from the database.
bool match = Hash::check(plain, old_hashed);
if (match && Hash::needs_rehash(old_hashed)) {
  string new_hashed = Hash::make(plain);
  //Store the new one in the database overwriting old_hashed
} else if (match) {
  // The user is authenticated
} else {
  // The user is not authenticated
}
```

Password hashing for the Ljus Framework is done using the Argon2 algorithm. This section should not be construed to be the one to use to verify file integrity, for that use Filesystem::hash().

Under the hood, the [reference argon2 implentation](https://github.com/P-H-C/phc-winner-argon2) is used. Our C++ wrapper fetches a secure salt from your system's '/dev/urandom', and will automatically handle salting throughout, simply work with the strings you get on either end of the equation.

### Hash::make
Parameter | Description
--------- | -----------
string plain | The plain text value

Return Type | Description
----------- | -----------
std::string | The hashed value


### Hash::check
Parameter | Description
--------- | -----------
string plain | The plain text value
string hashed | The salted hash, normally stored in a database


Return Type | Description
----------- | -----------
bool | If they match

### Hash::needs_rehash
Parameter | Description
--------- | -----------
string hashed | The hashed value, to check if it needs rehashing

Return Type | Description
----------- | -----------
bool | If it needs rehashing


<aside class="success">
Be sure to always check if a password needs rehashing, this helps ensure that you won't end up with passwords that aren't properly secured.
</aside>

## Encryption
> To encrypt strings, use the following approach:

```cpp
#include "ljus/encryption/Crypt.h"

std::string plain = "encrypt me";
std::string cipher_text = Crypt::encrypt(plain); //Base64 encoded
// Do something with your encrypted text 
```

> To decrypt them, use the following:

```cpp
// Now read it
std::string decrypted = Crypt::decrypt(cipher_text);
bool plain.compare(decrypted) == 0; // true
```

> To encrypt anything else, simply convert to bytes, and then make a string:

```cpp
size_t len;
unsigned char* bytes;
std::string stringified( reinterpret_cast<char const*>(bytes), len);
```

A component to encrypt strings (and anything you can turn into a string), using LibSodium's implementation of the ChaCha20-Poly1305 algorithm.

This is in a fairly stable external state and has been extensively tested using American Fuzzy Lop, however, is still not yet considered production ready.

The api looks slightly different from Laravel's, and the internals significantly so. Crypt exposes two methods:

<aside class="warning">Never use this with passwords, or any other information that needs to be irreversable.</aside>

### Crypt::encrypt
Parameter | Description
--------- | -----------
string plain | The plain text value

Return Type | Description
----------- | -----------
string | The encrypted value


### Crypt::decrypt
Parameter | Description
--------- | -----------
string encrypted | The encrypted value

Return Type | Description
----------- | -----------
string | The decrypted value

