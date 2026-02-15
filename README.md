# qq


[![Go](https://github.com/JFryy/qq/actions/workflows/go.yml/badge.svg)](https://github.com/JFryy/qq/actions/workflows/go.yml)
[![Docker Build](https://github.com/JFryy/qq/actions/workflows/docker-image.yml/badge.svg)](https://github.com/JFryy/qq/actions/workflows/docker-image.yml)
[![Go Version](https://img.shields.io/github/go-mod/go-version/JFryy/qq)](https://golang.org/)
[![License](https://img.shields.io/github/license/JFryy/qq)](https://github.com/JFryy/qq/blob/main/LICENSE)
[![Latest Release](https://img.shields.io/github/v/release/JFryy/qq)](https://github.com/JFryy/qq/releases)
[![Docker Pulls](https://img.shields.io/docker/pulls/jfryy/qq)](https://hub.docker.com/r/jfryy/qq)


`qq` is an interoperable configuration format transcoder with `jq` query syntax powered by `gojq`. `qq` is multi modal, and can be used as a replacement for `jq` or be interacted with via 
a REPL with autocomplete and realtime rendering preview for building queries.


`qq` is designed to support input output operations on a large variety of structured data codecs with the power of jq, please refer to the below for supported formats/extensions.


## Usage


Here's some example usage, this emphasizes the interactive mode for demonstration, but `qq` is designed for usage in shell scripts.
![Demo GIF](docs/demo.gif)


```sh
# JSON is default in and output.
cat file.${ext} | qq -i ${ext}


# Extension is parsed, no need for input flag
qq '.' file.xml


# random example: query xml, grep with gron using qq io and output as json
qq file.xml -o gron | grep -vE "sweet.potatoes" | qq -i gron


# get some content from a site with html input
curl motherfuckingwebsite.com | bin/qq -i html '.html.body.ul.li[0]'


# interactive query builder mode on target file
qq . file.json --interactive


# streaming mode - works with JSON, YAML, CSV and more
qq --stream 'select(length == 2)' large.json


# slurp mode - read multiple inputs into an array
echo -e '{"id":1}\n{"id":2}' | qq -s 'map(.id)'


# exit-status - use in conditionals
echo '{"active":true}' | qq -e '.active' && echo "is active"
```

## Git

You can also use it for cleaner diffing of configuration files by adding to your `git/config` file a snippet such as

```
  [diff "csv"]
  textconv = "f(){ in=\"$1\"; \
      if command -v qq   >/dev/null 2>&1; then qq --monochrome-output --output gron --input csv  \"$in\" 2>/dev/null | sort && exit 0; fi; \
      cat \"$in\"; \
    }; f"
  [diff "env"]
    textconv = "f(){ qq --monochrome-output --output gron --input env  \"$1\" 2>/dev/null | sort || cat \"$1\"; }; f"
  [diff "html"]
    textconv = "f(){ qq --monochrome-output --output gron --input html \"$1\" 2>/dev/null | sort || cat \"$1\"; }; f"
  [diff "ini"]
    textconv = "f(){ qq --monochrome-output --output gron --input ini  \"$1\" 2>/dev/null | sort || cat \"$1\"; }; f"
  [diff "toml"]
    textconv = "f(){ qq --monochrome-output --output gron --input toml \"$1\" 2>/dev/null | sort || cat \"$1\"; }; f"
```

and to `git/attributes` correspondingly

```
*.csv diff=csv
*.env diff=env
*.html diff=html
*.ini diff=ini
*.toml diff=toml
```

## Installation


From brew:


```shell
brew install jfryy/tap/qq 
```


From [AUR](https://aur.archlinux.org/packages/qq-git) (ArchLinux):


```shell
yay qq-git
```


From source (requires `go` `>=1.22.4`)
```shell
make install
```


Download at releases [here](https://github.com/JFryy/qq/releases).
