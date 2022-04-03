# git-merge-driver

## Installation

You can config the drivers in one of the two options:
- Inline Config.
- External shell script.

Choose as you wish.

---

### `keepTheirs` 
> [Guide](https://stackoverflow.com/a/27999588/14427765)

#### Inline Config:

Run 
```
git config merge.keepTheirs.driver "cp -f %B %A"
```
So your config file will look like this:
```
[merge "keepTheirs"]
	driver = cp -f %B %A
```
Afterwards, navigate to you `.gitattributes` and, for example, use this driver on all `.txt` files:
```
*.txt merge=keepTheirs
```

#### External shell script:

There is an option to create as an external shell file, and then execute it.
  
Run:
```
git config merge.keepTheirs.driver "./keepTheirs.sh %O %A %B"
```

So your config file will look like this:
```
[merge "keepTheirs"]
        driver = ./keepTheirs.sh %O %A %B
```

Create a `keepTheirs.sh`:
```sh
#!/bin/sh
cp -f $3 $2
exit 0
```

Afterwards, navigate to you `.gitattributes` and, for example, use this driver on all `.txt` files:
```
*.txt merge=keepTheirs
```
---

### `keepMine`
> [Guide](https://stackoverflow.com/a/930495/14427765)

#### Inline Config:

Run:
```
git config merge.keepMine.driver true 
```

So your config file will look like this:
```
[merge "keepMine"]
	driver = true
```

Afterwards, navigate to you `.gitattributes` and, for example, use this driver on all `.txt` files:
```
*.txt merge=keepMine
```

#### External shell script:

Run:
```
git config merge.keepMine.driver "./keepMine.sh %O %A %B"
```

So your config file will look like this:
```
[merge "keepMine"]
        driver = ./keepMine.sh %O %A %B
```

Create a `keepMine.sh`:
```sh
#!/bin/sh
# I want to keep MY version when there is a conflict
# Nothing to do: %A (the second parameter) already contains my version
# Just indicate the merge has been successfully "resolved" with the exit status
exit 0
```

Afterwards, navigate to you `.gitattributes` and, for example, use this driver on all `.txt` files:
```
*.txt merge=keepMine
```

## How To Use

1. Choose your preferred driver when resolving a conflict automatically:
    - `keepTheirs`: Keeps their version, and overrides yours.
    - `keepMine`: Keeps your version, and overrides theirs.
    
    and add them as the `.gitattribute` strategy.

1. Run:
    ```
    git checkout master
    git merge feature-branch
    ```
    and the driver you have picked will resolve your conflicts automatically.
