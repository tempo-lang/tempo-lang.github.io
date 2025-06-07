# Website for Tempo

This repository contains the website along with all documentation for Tempo.

## Local Development

Pre-requisites: [Hugo](https://gohugo.io/getting-started/installing/), [Go](https://golang.org/doc/install) and [Git](https://git-scm.com)

```shell
# Clone the repo
git clone https://github.com/tempo-lang/tempo-lang.github.io.git

# Change directory
cd tempo-lang.github.io

# Start the server
hugo mod tidy
hugo server
```

### Update theme

```shell
hugo mod get -u
hugo mod tidy
```

See [Update modules](https://gohugo.io/hugo-modules/use-modules/#update-modules) for more details.
