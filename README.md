## ...?
Ever came across a dumb program which keeps running the same (slow) command
over and over, expecting different output, but you know that the output
couldn't possibly ever change?

Place a symlink to cache-wrap early in your `$PATH`, give it the
name of the command you want to cache, and then just call the
command as usual.
cache-wrap will run the next command in `$PATH` that has the
same name as the symlink, and stick the stdout into memcache.
The next time the same command is ran from the same directory with
the same arguments, the program will not run and instead the result
will come from memcache.

## examples
```
$ # (the script is in ~/cache-path/cache-wrap
$ ALT_PATH=$HOME/cache-path
$ ln -s ~/cache-path/cache-wrap $ALT_PATH/date
$ date
[...]
$ date
[same output as above, from memcache]
```

**Note**: if the command writes anything to STDERR then its STDOUT will **NOT** be cached.

## options
You can set the environment variable `CACHEWRAP_OPTS` to a
comma-separated list of options:

 - `help`         print this text and exit
 - `debug`        output some debugging information
 - `server`       specify Memcache server address (default `127.0.0.1:11211`)
 - `nocache`      disable cache, always run actual command
 - `clear`        clear cache (delete from Memcache) then run the actual command
 - `nuke`         alias for `clear`

#### using options
```
$ CACHEWRAP_OPTS=server=2.3.4.5:11211,debug slow-cmd arg1 arg2
```
