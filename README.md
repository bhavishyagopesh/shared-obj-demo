## Different methods to prepare a shared-object(.so):

1. Using `$LD_LIBRARY_PATH` env variable:

```bash

gcc -c -Wall -Werror -fpic foo.c 
# -c Only compile, -Wall [with warnings](https://gcc.gnu.org/onlinedocs/gcc/Warning-Options.html#index-Wall), -Werror report errors, -fpic [position independent code](https://stackoverflow.com/questions/5311515/gcc-fpic-option)(like without compiler optimizations)

gcc -shared -o libfoo.so foo.o
#create a shared-library 

gcc -L /path/to/libfoo -Wall -o final main.c -lfoo
# -L gives path to shared object

export LD_LIBRARY_PATH=/path/to/libfoo
#sets LD_LIBRARY_PATH env-variable

./final
```

Temporary output->

```text
This is a shared obj...
This is a shared function
```

2. Using rpath:

```bash
gcc -L /path/to/libfoo -Wl,-rpath=/path/to/libfoo -Wall -o final main.c -lfoo
./final
```

Temporary output->
```text
This is a shared obj...
This is a shared function
```


3. Adding it to `/usr/lib/`:

```bash
cp /path/to/libfoo/libfoo.so /usr/lib
chmod 0755 /usr/lib/libfoo.so
gcc -Wall -o final  main.c -lfoo
./final
```

Temporary output->
```text
This is a shared obj...
This is a shared function
```

