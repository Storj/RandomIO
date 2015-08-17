RandomIO
===============

[![Build Status](https://travis-ci.org/Storj/RandomIO.svg)](https://travis-ci.org/Storj/RandomIO) [![Coverage Status](https://img.shields.io/coveralls/Storj/RandomIO.svg)](https://coveralls.io/r/Storj/RandomIO?branch=master)

`RandomIO` provides a readable interface for cryptographic quality random bytes.  It also allows for generation of random files, dumping random bytes to files, and a `.read()` method for reading bytes.

### Installation

```
git clone https://github.com/storj/RandomIO
cd RandomIO
pip install .
```

### File generation

Generate a 50 byte file from a seed with one line:

```python
import RandomIO

path = RandomIO.RandomIO('seed string').genfile(50)
with open(path,'rb') as f:
	print(f.read())

# b"\xec\xf4C\xeb\x1d\rU%\xca\xae\xa4^=*in\x90y\x12\x86\xce\xe5N\xce-\x16
#   \xc8r\x83sh\xdfp\xb7\xbb\xc2\x04\x11\xda)\xc1*_\x01\xe5\xd8\x0f}N0"
```

It is possible to specify the directory to generate the file in, or the file name:

```python
# specify a directory:

path = RandomIO.RandomIO('seed string').genfile(100,'dir/')
print(path)

# 'dir/22aae6183b5202cd0c74381c673394d2'

# or file name:

path = RandomIO.RandomIO('seed string').genfile(100,'dir/file')
print(path)

# 'dir/file'
```

### Byte generation

It is possible to read random bytes and dump those bytes to a file object:

```python
import RandomIO

s = RandomIO.RandomIO()
print(s.read(10))

# b'\x8bfT\x9c\x06_)\xa2,\xd0'

# or generate seeded random bytes
s = RandomIO.RandomIO('seed string')
print(s.read(10))

# b'\xec\xf4C\xeb\x1d\rU%\xca\xae'

# dump the bytes into a file object
s = RandomIO.RandomIO('seed string')
with open('path/to/file','wb') as f:
	s.dump(10,f)

with open('path/to/file','rb') as f:
	print(f.read())
	
# b'\xec\xf4C\xeb\x1d\rU%\xca\xae'
```

### Performance

```
> python -m timeit -p -s 'import RandomIO, os' 'path=RandomIO.RandomIO().genfile(100000000);os.remove(path)'
10 loops, best of 3: 1.4 sec per loop
```

From a simple timeit analysis on a 2.4 GHz PC it can generate files at around 70 MB/s.
