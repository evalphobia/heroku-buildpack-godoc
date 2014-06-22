# Heroku Buildpack: Godoc

Much of this code has been copied and stripped down from the very helpful [heroku-buildpack-go](https://github.com/kr/heroku-buildpack-go) project.

## Example

```
$ ls -A1
.git
.godoc
Procfile

$ cat .godoc
github.com/timehop/redisent

$ cat Procfile
web: web

$ heroku create -b https://github.com/kevin-cantwell/heroku-buildpack-godoc
...

$ git push heroku master
Fetching repository, done.
Counting objects: 2, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 229 bytes | 0 bytes/s, done.
Total 2 (delta 1), reused 0 (delta 0)

-----> Fetching custom git buildpack... done
=====> Downloading Buildpack: https://github.com/kevin-cantwell/heroku-buildpack-godoc
=====> Detected Framework: Godoc
       Installing Virtualenv..... done
       Installing Mercurial.... done
       Installing Bazaar.... done
-----> Using go1.2 (Specify a different goversion with GOVERSION=1.x)
-----> Running: go get -d -u github.com/timehop/redisent
-----> Creating executable: /app/bin/web
-----> Discovering process types
       Procfile declares types -> web
-----> Compressing... done, 5.0MB
-----> Launching... done, v6
       http://stinky-poop-3607.herokuapp.com/ deployed to Heroku
```

The buildpack will detect your repository as a godoc server if it contains a `.godoc` hidden file.

The `.godoc` file should list at least one go gettable package.

The buildpack will compile a small executable named `web` that will be used indirectly to launch 
the godoc http server. Thus, your Procfile must contain a `web: web` line.

By default, the buildpack will download and use go1.3. You may override the version by configuring GOVERSION:

`$ heroku config:set GOVERSION=1.2.1`

