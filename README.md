## apt-get-common_issues
Following are some common errors that may arise while running `$ apt update`.

It is very easy to upgrade and manage software and packages with systems based on Debian or Ubuntu using `$ apt update` or `$ apt-get update`. But sometimes it can be rather painful.

### Solving an expired key (KEYEXPIRED) with apt

* Full list of known keys can be viewed using:
```
$ apt-key list
```

* List only *expired* repository keys:
```
$ apt-key list | grep "expired"
```
* Update the gpg key. Run:
```
$ sudo apt-key adv --keyserver keys.gnupg.net --recv-keys <KEY>
```
* Update
After renewing the expired key, `apt update` can be run again an you can install any available upgrades
```
$ sudo apt-get update && apt-get upgrade
```
