**Work in progress**

# devenv

Development environment MARK III

# TL;DR

```
./script/bootstrap # clones to 'workspace/'
vagrant up
vagrant ssh
fig -f alpha/fig.yml up
```

## Roll your own

If you want only middleware, for instance, and want to run apps locally...

You can copy any one of [alpha](alpha) or [concept](concept), remove apps, but leave Redis/ElasticSearch/PostgresQL, and bootstrap it.

```
cp -R alpha foo
# make changes
./script/bootstrap foo
# and then in the VM...
fig -f foo/fig.yml up
```


# Hot reload

For now, the dev env won't pick up changes automatically. 

Change your code, then restart ```fig``` or do a "clean" with:

1. Ensure all containers are stopped
2. Remove containers with

```
docker rm $(docker ps --no-trunc -aq)
```

3. Remove Land Registry images with
 
```
docker rmi <uuid of image>
```

Remove all images with

```
docker rmi $(docker images --no-trunc -q)
```

4. Rebuild the images with

```
fig -f alpha/fig.yml build
```

5. Bring fig up with

```
fig -f alpha/fig.yml up
```

**(someone, please verify these for me)**
