# Installing libgraphqlparser on Heroku
This buildpack will install `libgraphqlparser` on Heroku.

## Usage
Getting `libgraphqlparser` working on Heroku is a pretty big pain. But these steps will hopefully help.


### Configure buildpacks
Set your app to use [heroku-buildpack-multi](https://github.com/ddollar/heroku-buildpack-multi).

Then create a file in your repo root called `.buildpacks`. You need to add this to the beginning:
```
https://github.com/ddollar/heroku-buildpack-apt
https://github.com/goco-inc/heroku-buildpack-libgraphqlparser.git
```

Next, create a file in your repo root called `Aptfile`, and put this in it:
```
cmake
flex
```

## Using the `graphql-libgraphqlparser` gem
I created this buildpack so that I could use the [`graphql-libgraphqlparser`](https://github.com/rmosolgo/graphql-libgraphqlparser-ruby) gem at GoCo. To get that gem to work, you need a few more things:

### Heroku Bundle Config
You need to add the [`heroku-bundle-config`](https://github.com/timolehto/heroku-bundle-config) buildpack to your app. Here's a complete `.buildpacks` file example:
```
https://github.com/ddollar/heroku-buildpack-apt
https://github.com/goco-inc/heroku-buildpack-libgraphqlparser.git
https://github.com/timolehto/heroku-bundle-config.git
https://github.com/heroku/heroku-buildpack-ruby
```

Then create a file in your repo root called `.heroku-bundle/config`, and put this in it:
```
---
BUNDLE_BUILD__GRAPHQL-LIBGRAPHQLPARSER: "--with-graphql-include=/app/libgraphqlparser/include/graphqlparser
  --with-graphql-lib=/app/libgraphqlparser/lib"
```

That should be all you need!
