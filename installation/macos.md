# macOS

### Editions  <a href="#editions" id="editions"></a>

Hugo is available in two editions: standard and extended. With the extended edition you can:

* Encode to the WebP format when [processing images](https://gohugo.io/content-management/image-processing/). You can decode WebP images with either edition.
* [Transpile Sass to CSS](https://gohugo.io/hugo-pipes/transpile-sass-to-css/) using the embedded LibSass transpiler. The extended edition is not required to use the [Dart Sass](https://gohugo.io/hugo-pipes/transpile-sass-to-css/#dart-sass) transpiler.

We recommend that you install the extended edition.

### Prerequisites  <a href="#prerequisites" id="prerequisites"></a>

Although not required in all cases, [Git](https://git-scm.com/), [Go](https://go.dev/), and [Dart Sass](https://gohugo.io/hugo-pipes/transpile-sass-to-css/#dart-sass) are commonly used when working with Hugo.

Git is required to:

* Build Hugo from source
* Use the [Hugo Modules](https://gohugo.io/hugo-modules/) feature
* Install a theme as a Git submodule
* Access [commit information](https://gohugo.io/variables/git) from a local Git repository
* Host your site with services such as [CloudCannon](https://cloudcannon.com/), [Cloudflare Pages](https://pages.cloudflare.com/), [GitHub Pages](https://pages.github.com/), [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/), and [Netlify](https://www.netlify.com/)

Go is required to:

* Build Hugo from source
* Use the Hugo Modules feature

Dart Sass is required to transpile Sass to CSS when using the latest features of the Sass language.

Please refer to the relevant documentation for installation instructions:

* [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Go](https://go.dev/doc/install)
* [Dart Sass](https://gohugo.io/hugo-pipes/transpile-sass-to-css/#dart-sass)

### Prebuilt binaries  <a href="#prebuilt-binaries" id="prebuilt-binaries"></a>

Prebuilt binaries are available for a variety of operating systems and architectures. Visit the [latest release](https://github.com/gohugoio/hugo/releases/latest) page, and scroll down to the Assets section.

1. Download the archive for the desired [edition](https://gohugo.io/installation/macos/#editions), operating system, and architecture
2. Extract the archive
3. Move the executable to the desired directory
4. Add this directory to the PATH environment variable
5. Verify that you have _execute_ permission on the file

Please consult your operating system documentation if you need help setting file permissions or modifying your PATH environment variable.

If you do not see a prebuilt binary for the desired edition, operating system, and architecture, install Hugo using one of the methods described below.

### Package managers  <a href="#package-managers" id="package-managers"></a>

#### Homebrew  <a href="#homebrew" id="homebrew"></a>

[Homebrew](https://brew.sh/) is a free and open source package manager for macOS and Linux. This will install the extended edition of Hugo:

```sh
brew install hugo
```

#### MacPorts  <a href="#macports" id="macports"></a>

[MacPorts](https://www.macports.org/) is a free and open source package manager for macOS. This will install the extended edition of Hugo:

```sh
sudo port install hugo
```

### Docker  <a href="#docker" id="docker"></a>

[Erlend Klakegg Bergheim](https://github.com/klakegg) graciously maintains [Docker images](https://hub.docker.com/r/klakegg/hugo) based on images for Alpine Linux, Busybox, Debian, and Ubuntu.

```sh
docker pull klakegg/hugo
```

### Build from source  <a href="#build-from-source" id="build-from-source"></a>

To build the extended edition of Hugo from source you must:

1. Install [Git](https://git-scm.com/)
2. Install [Go](https://go.dev/) version 1.19 or later
3. Install a C compiler, either [GCC](https://gcc.gnu.org/) or [Clang](https://clang.llvm.org/)
4. Update your `PATH` environment variable as described in the [Go documentation](https://go.dev/doc/code#Command)

> The install directory is controlled by the `GOPATH` and `GOBIN`environment variables. If `GOBIN` is set, binaries are installed to that directory. If `GOPATH` is set, binaries are installed to the bin subdirectory of the first directory in the `GOPATH` list. Otherwise, binaries are installed to the bin subdirectory of the default `GOPATH` (`$HOME/go` or `%USERPROFILE%\go`).

Then build and test:

```sh
CGO_ENABLED=1 go install -tags extended github.com/gohugoio/hugo@latest
hugo version
```

### Comparison  <a href="#comparison" id="comparison"></a>

|                           | Prebuilt binaries | Package managers                                   | Docker                                            | Build from source |   |
| ------------------------- | ----------------- | -------------------------------------------------- | ------------------------------------------------- | ----------------- | - |
| Easy to install?          | ✔️                | ✔️                                                 | ✔️                                                | ✔️                |   |
| Easy to upgrade?          | ✔️                | ✔️                                                 | ✔️                                                | ✔️                |   |
| Easy to downgrade?        | ✔️                | ✔️ [1](https://gohugo.io/installation/macos/#fn:1) | ✔️                                                | ✔️                |   |
| Automatic updates?        | ❌                 | ❌ [2](https://gohugo.io/installation/macos/#fn:2)  | ❌ [2](https://gohugo.io/installation/macos/#fn:2) | ❌                 |   |
| Latest version available? | ✔️                | ✔️                                                 | ✔️                                                | ✔️                |   |

***

1. Easy if a previous version is still installed. [↩︎](https://gohugo.io/installation/macos/#fnref:1)
2. Possible but requires advanced configuration. [↩︎](https://gohugo.io/installation/macos/#fnref:2) [↩︎](https://gohugo.io/installation/macos/#fnref1:2)
