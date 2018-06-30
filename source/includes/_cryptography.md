
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
}
// The user is authenticated
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
