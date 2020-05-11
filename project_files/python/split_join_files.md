## Functions to Split and Join Large Files

Let's suppose that you have a large file that you need to break into smaller parts. Here you have 2 functions that do that. The first function, `split` splits the file by the size of a defined chunk. The second function puts them together.

```python
import sys, os

m = 80 # How many megabytes each chunk shall be?
kilobytes = 1024
megabytes = kilobytes * 1000
chunksize = int(m * megabytes)

def split(fromfile, todir, chunksize=chunksize):
    if not os.path.exists(todir):
        os.mkdir(todir)
    else:
        for fname in os.listdir(todir):
            os.remove(os.path.join(todir, fname))
    partnum = 0
    input = open(fromfile, 'rb')
    while 1:
        chunk = input.read(chunksize)
        if not chunk: break
        partnum  = partnum+1
        filename = os.path.join(todir, ('part%04d' % partnum))
        fileobj  = open(filename, 'wb')
        fileobj.write(chunk)
        fileobj.close()
    input.close(  )


readsize = 1024

def join(fromdir, tofile):
    output = open(tofile, 'wb')
    parts  = os.listdir(fromdir)
    parts.sort(  )
    for filename in parts:
        filepath = os.path.join(fromdir, filename)
        fileobj  = open(filepath, 'rb')
        while 1:
            filebytes = fileobj.read(readsize)
            if not filebytes: break
            output.write(filebytes)
        fileobj.close(  )
    output.close(  )

#split('./nf/test.txt', './nf/test/')
#join('./nf/test/', 'tofile.txt')
```