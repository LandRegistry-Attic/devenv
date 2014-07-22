**Work in progress**

# devenv

Development environment MARK III

# How?

**for all commands below, you can ```s/alpha/concept/g```**

1. Bootstrap

```
./script/bootstrap 
```

## Need a subset?

You can copy any one of [alpha](alpha) or [concept](concept), modify it, and bootstrap it.

    cp -R alpha thingy
    # hack away, perhaps leave only Redis, ElasticSearch and PostgresQL, but run apps locally...
    ./script/bootstrap thingy

2. Vagrant

```
vagrant up
vagrant ssh
fig -f alpha/fig.yml up
```

```workspace``` will contain clones of your projects, and you can make changes there.

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
