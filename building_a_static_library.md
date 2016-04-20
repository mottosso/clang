### Building a Static Library

Starting from the bottom up, we'll build a library and then use that library in the next example.

We will have 3 files in this layout.

```
| Layout              | Description
|---------------------|----------------------------------
| root/               |
|   include/          |
|     lib.h           | 1. Definition of a sample function
|   src/              |
|     lib.cpp         | 2. Declaration of function
|     main.cpp        | 3. Use of library
|   build/            |
|     ...             | Output goes here
```

Here is the content of each file.

**lib.h**

```cpp
namespace mynamespace
{
    void helloWorld();
}
```

**lib.cpp**

```cpp
#include <iostream>
#include "lib.h"

namespace mynamespace
{
    void helloWorld()
    {
        std::cout << "Hello World!" << std::endl;
    }
}
```

**main.cpp**

```cpp
#include "lib.h"

int main(void)
{
    mynamespace::helloWorld();
    return(0);
}
```

<br>
<br>
<br>

#### Compile

**build-lib.sh**

This will output `lib/lib.lib`.

```bat
#!/usr/bin/env bash

mkdir -p build
pushd build

clang++ \
	-c \
	-Wall \
	-o lib.o \
	-I ../include \
	../src/lib.cpp && \

llvm-ar \
	rc ../lib/lib.a \
	lib.o

popd
```

**build.sh**

This will use `lib/lib.lib` to compile a console application.

```bat
#!/usr/bin/env bash

mkdir -p build
pushd build

clang++ \
	-Wall \
	-o main \
	-I ../include \
	../src/main.cpp \
	../lib/lib.a

popd
```

<br>
<br>
<br>

### Running

Now that you are done, you can run your program.

```bat
$ build\main
Hello World!
```

This program is:

1. Calling a function defined in `lib.h`.
2. Which is defined in `lib.cpp`
3. And provided to our application via `lib.lib`

You will note that if you excluded `lib.lib` from your `build.bat` script, an error is thrown.

```bat
/tmp/main-05a698.o: In function `main':
../src/main.cpp:(.text+0x10): undefined reference to `mynamespace::helloWorld()'
clang: error: linker command failed with exit code 1 (use -v to see invocation)
```