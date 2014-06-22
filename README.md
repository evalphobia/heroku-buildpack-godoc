# Heroku Buildpack: Godoc

*Much of this code has been copied and stripped down from the very 
helpful [heroku-buildpack-go](https://github.com/kr/heroku-buildpack-go) project.*

This buildpack lets you run a godoc http server on Heroku.

The buildpack will detect your repository as a godoc server if it contains a 
`.godoc` hidden file. The `.godoc` file should list at least one go-gettable package.

The buildpack will compile a small executable named `web` that will be used indirectly to launch 
the godoc http server. Thus, your Procfile must contain a `web: web` line.

By default, the buildpack will download and use go1.3. You may override the go version 
by setting the GOVERSION var: `$ heroku config:set GOVERSION=1.2.1`

## Example

```
$ ls -A1
.git
.godoc
Procfile

$ cat .godoc
github.com/garyburd/redigo/redis

$ cat Procfile
web: web

$ heroku create -b https://github.com/kevin-cantwell/heroku-buildpack-godoc
Creating lit-shelf-8223... done, stack is cedar
BUILDPACK_URL=https://github.com/kevin-cantwell/heroku-buildpack-godoc
http://lit-shelf-8223.herokuapp.com/ | git@heroku.com:lit-shelf-8223.git

$ git push heroku master
Initializing repository, done.
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (7/7), 590 bytes | 0 bytes/s, done.
Total 7 (delta 0), reused 0 (delta 0)

-----> Fetching custom git buildpack... done
-----> Godoc app detected
       Installing Virtualenv..... done
       Installing Mercurial.... done
       Installing Bazaar.... done
-----> Installing https://storage.googleapis.com/golang/go1.3.linux-amd64.tar.gz...... done
-----> Running: go get -d -u github.com/garyburd/redigo/redis
-----> Creating executable: /app/bin/web
-----> Discovering process types
       Procfile declares types -> web

-----> Compressing... done, 49.3MB
-----> Launching... done, v4
       http://lit-shelf-8223.herokuapp.com/ deployed to Heroku

```

