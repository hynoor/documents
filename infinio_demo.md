
# Infinio Demo

## _Actions_

### open

Open user specified percentages of existing files and holding file handles for given duration
>--action=open {--directory=} {--open-strategy=write:300} [--file-number=.100]

Prepare 10 files for open:
```
python infinio.py --directory=/mnt/nfv/demotree --file-number=10 --action=create
```
Open with _read_ mode, then keep all the opens for 1000 seconds:
```
python infinio.py --directory=/mnt/nfv/demotree --action=open --open-strategy=read:1000
```

examine the open status on array:
```
/nas/bin/.server_config [nas_server_name] -v "lockd list"
```

### move

Move user specified percentages of existing files to user specific destination directory
>--action=move    {--directory=} {--dest-directory=}    [--file-number=.100]

Move all file within a tree (demotree) to another (demotree2) then do vice versa
```
python infinio.py --directory=/mnt/nfv/demotree#/mnt/nfv/demotree2 --dest-directory=/mnt/nfv/demotree2#/mnt/nfv/demotree --action=move#move
```

### copy
>--action=copy {--directory=} {--dest-directory=} [--file-number=.100]

Copy 50% of files of a tree to another tree:
```
python infinio.py --directory=/mnt/nfv/demotree --dest-directory=/mnt/nfv/demotree2 --action=copy --file-number=.50
```

## Parameters

### name-seed
The seed of file names to be generated, accepts any characters, include those beyond of ASCII. Random
ascii strings is adopted by default.

Create a set of files which name is composed of only numbers 12345 and has 20 length
```
python infinio.py --directory=/mnt/nfv/demotree --name-seed=12345 --name-length=10 --file-number=100 --action=create
```


### seek-type (io-mode is obsoleted) 
Seek type of within creation of each file, valid values are [forth, back, random], which way
controls the direction of each write

![seektype](https://github.com/hynoor/image_repository/blob/master/seek-type.png?raw=true)

Create a big file with _random_ seek-type:
```
python infinio.py --directory=/mnt/nfv/demotree --seek-type=random --file-size=50m --action=write --data-pattern=random
```

### data-pattern
Some demos for popular data-pattern usages

Create sparse files with 99% sparse ratio:
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --file-size=1G --data-pattern=sparse:99
```

Create files with binary zero data pattern:
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=ZERO
```
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=0x00
```

Create files with binary one data pattern:
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=ONE --block-size=8k
```
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=0xff --block-size=8k
```

Create file with compressible data pattern (50% compressible data):
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=compress:50 --block-size=8k
```
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=random+ZERO --block-size=4k+4k
```

Create file with bit mode, which composed of all binary one except one bit of zero in the middle of the block
```
python infinio.py --directory=/mnt/nfv/demotree --action=create --data-pattern=ONE+bit:00001000+ONE --block-size=4k+1b+4095b
```

### byte-range locks

Demos for file locking
