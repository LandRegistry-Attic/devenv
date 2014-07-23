:skull: *Work in progress* :skull:

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

*For now, the dev env won't pick up changes automatically.*

Change your code, then restart ```fig``` or do a "clean" with:

- Ensure all containers are stopped
- Remove containers with

```
docker rm $(docker ps --no-trunc -aq)
```

- Remove Land Registry images with
 
```
docker rmi <uuid of image>
```

Remove all images with

```
docker rmi $(docker images --no-trunc -q)
```

- Rebuild the images with

```
fig -f alpha/fig.yml build
```

- Bring fig up with

```
fig -f alpha/fig.yml up
```

**(someone, please verify these for me)**

# Extending

You can add a new app to your existing VM without having to start over.

Here I show an example of adding ```property-frontend``` to ```fig.yml```, with all the other apps pre-existing, upon which it's merely a case  of running ```fig up``` again, but with the ```--no-recreate``` option so that your existing images aren't re-built.


```
vagrant@precise64:/vagrant$ fig -f alpha/fig.yml up --no-recreate
Starting new HTTP connection (1): 0.0.0.0
Starting alpha_publictitlesdb_1...
Starting alpha_redis_1...
Starting alpha_sysofrecdb_1...
Starting alpha_elasticsearch_1...
Starting alpha_searchapi_1...
Starting alpha_systemofrecord_1...
Starting alpha_mint_1...
Starting alpha_publictitlesapi_1...
Creating alpha_propertyfrontend_1...
Building propertyfrontend...
 ---> cdc18ed4ed6f
 Step 1 : ADD . /code
  ---> 8f7b87da9c97

<snippity snip>
```

## I added something, but made a mistake

If you add a new app, but made a mistake, or omitted something, you'll see a message like:

```
Service 'caseworkfrontend' failed to build: ... <snip>
```

The command ```docker ps -a``` will then show a half-baked image with an interim name with a non-zero exit status:

![half-baked](http://i.imgur.com/1e5QY6x.png)

The trick here is to delete the container and image for the offending image, fix the code, then ```fig up``` again.

![go go go](http://i.imgur.com/wHDm6c8.png)

# Something went wrong

Sometimes Docker or something more sinister throws a wobbly, and stopping/killing/rf-minus-effing it doesn't cut the mustard.

Tab to a different shell and ```vagrant reload``` the VM.

Then ```vagrant ssh``` in again, try ```fig up``` again, or delete the containers/images before trying.

# Remove <none> containers

```
docker rmi $(docker images | grep "^<none>" | awk '{print $3}')
```
