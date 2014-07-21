# devenv

Development environment MARK III

# How?

**for all commands below, you can ```s/alpha/concept/g```**

1. Bootstrap

```
./script/bootstrap alpha
```

2. Vagrant

```
vagrant up
vagrant ssh
fig -f up alpha/fig.yml
```

```workspace``` will contain clones of your projects, and you can make changes there.

# Hot reload

For now, the dev env won't pick up changes automatically. 

Change your code, then restart ```fig``` in the VM.
