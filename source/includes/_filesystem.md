#Filesystem

> Basic usage

```cpp
#include "ljus/files/Filesystem.h"

string content = "your content";

//Store the contents
Filesystem::put("/tmp/my_file.txt", content);

string fetched = Filesystem::get("/tmp/my_file.txt");

fetched == content; //True, normally at least.

long long size = Filesystem::size("/tmp/my_file.txt"); 

size == 12L; // Should be true

Filesytem::hash("/tmp/my_file.txt") == "u6vt2M..."; // True, the blake2b hash
```

> Utility functions

```cpp
#include "ljus/files/Filesystem.h"

bool exists = Filesystem::exists("/tmp/i_exist.txt");
```

Ljus' filesystem is designed to be easy to use, exceedingly fast, with no dependencies other than [std::filesystem](https://en.cppreference.com/w/cpp/filesystem). For those coming from Laravel, it should look fairly similar. Right now, only one filesystem is supported (local disk). But an extension for a different file system from the `Ljus::Filesystem` class, can be made to support any disk-like thing.

