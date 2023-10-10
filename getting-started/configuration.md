---
description: How to configure your Hugo site.
---

# Configuration

### Configuration file  <a href="#configuration-file" id="configuration-file"></a>

Hugo uses the `hugo.toml`, `hugo.yaml`, or `hugo.json` (if found in the site root) as the default site configuration file.

The user can choose to override that default with one or more site configuration files using the command-line `--config`switch.

Examples:

```txt
hugo --config debugconfig.toml
hugo --config a.toml,b.toml,c.toml
```

Multiple site configuration files can be specified as a comma-separated string to the `--config` switch.

### hugo.toml vs config.toml  <a href="#hugotoml-vs-configtoml" id="hugotoml-vs-configtoml"></a>

In Hugo 0.110.0 we changed the default configuration base file name to `hugo`, e.g. `hugo.toml`. We will still look for `config.toml`etc., but we recommend you eventually rename it (but you need to wait if you want to support older Hugo versions). The main reason we’re doing this is to make it easier for code editors and build tools to identify this as a Hugo configuration file and project.

[New in v0.110.0](https://github.com/gohugoio/hugo/releases/tag/v0.110.0)

### Configuration directory  <a href="#configuration-directory" id="configuration-directory"></a>

In addition to using a single site configuration file, one can use the `configDir` directory (default to `config/`) to maintain easier organization and environment specific settings.

* Each file represents a configuration root object, such as `params.toml` for `[Params]`, `menu(s).toml` for `[Menu]`, `languages.toml` for `[Languages]` etc…
* Each file’s content must be top-level, for example:

hugo.yaml toml json&#x20;

```toml
[Params]
  foo = 'bar'
```

params.yaml toml json&#x20;

```toml
foo = 'bar'
```

* Each directory holds a group of files containing settings unique to an environment.
* Files can be localized to become language specific.

```txt
├── config
│   ├── _default
│   │   ├── hugo.toml
│   │   ├── languages.toml
│   │   ├── menus.en.toml
│   │   ├── menus.zh.toml
│   │   └── params.toml
│   ├── production
│   │   ├── hugo.toml
│   │   └── params.toml
│   └── staging
│       ├── hugo.toml
│       └── params.toml
```

Considering the structure above, when running `hugo --environment staging`, Hugo will use every setting from `config/_default` and merge `staging`’s on top of those.

Let’s take an example to understand this better. Let’s say you are using Google Analytics for your website. This requires you to specify `googleAnalytics = "G-XXXXXXXX"` in `hugo.toml`. Now consider the following scenario:

* You don’t want the Analytics code to be loaded in development i.e. in your `localhost`
* You want to use separate googleAnalytics IDs for your production & staging environments (say):
  * `G-PPPPPPPP` for production
  * `G-SSSSSSSS` for staging

This is how you need to configure your `hugo.toml` files considering the above scenario:

1. In `_default/hugo.toml` you don’t need to mention `googleAnalytics` parameter at all. This ensures that no Google Analytics code is loaded in your development server i.e. when you run `hugo server`. This works since, by default Hugo sets `Environment=development` when you run `hugo server` which uses the configuration files from `_default` folder
2.  In `production/hugo.toml` you just need to have one line:

    `googleAnalytics = "G-PPPPPPPP"`

    You don’t need to mention all other parameters like `title`, `baseURL`, `theme` etc. again in this configuration file. You need to mention only those parameters which are different or new for the production environment. This is due to the fact that Hugo is going to **merge** this on top of `_default/hugo.toml`. Now when you run `hugo` (build command), by default hugo sets `Environment=production`, so the `G-PPPPPPPP` analytics code will be there in your production website
3.  Similarly in `staging/hugo.toml` you just need to have one line:

    `googleAnalytics = "G-SSSSSSSS"`

    Now you need to tell Hugo that you are using the staging environment. So your build command should be `hugo --environment staging` which will load the `G-SSSSSSSS`analytics code in your staging website

Default environments are **development** with `hugo server`and **production** with `hugo`.

### Merge configuration from themes  <a href="#merge-configuration-from-themes" id="merge-configuration-from-themes"></a>

The configuration value for `_merge` can be one of:

noneNo merge.shallowOnly add values for new keys.deepAdd values for new keys, merge existing.

Note that you don’t need to be so verbose as in the default setup below; a `_merge` value higher up will be inherited if not set.

hugo.yaml toml json&#x20;

```toml
[build]
  _merge = 'none'
[caches]
  _merge = 'none'
[cascade]
  _merge = 'none'
[deployment]
  _merge = 'none'
[frontmatter]
  _merge = 'none'
[imaging]
  _merge = 'none'
[languages]
  _merge = 'none'
  [languages.en]
    _merge = 'none'
    [languages.en.menus]
      _merge = 'shallow'
    [languages.en.params]
      _merge = 'deep'
[markup]
  _merge = 'none'
[mediatypes]
  _merge = 'shallow'
[menus]
  _merge = 'shallow'
[minify]
  _merge = 'none'
[module]
  _merge = 'none'
[outputformats]
  _merge = 'shallow'
[outputs]
  _merge = 'none'
[params]
  _merge = 'deep'
[permalinks]
  _merge = 'none'
[privacy]
  _merge = 'none'
[related]
  _merge = 'none'
[security]
  _merge = 'none'
[server]
  _merge = 'none'
[services]
  _merge = 'none'
[sitemap]
  _merge = 'none'
[taxonomies]
  _merge = 'none'
```

### All configuration settings  <a href="#all-configuration-settings" id="all-configuration-settings"></a>

The following is the full list of Hugo-defined variables. Users may choose to override those values in their site configuration file(s).

#### archetypeDir  <a href="#archetypedir" id="archetypedir"></a>

**Default value:** “archetypes”

The directory where Hugo finds archetype files (content templates). Also see [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts) for an alternative way to configure this directory (from Hugo 0.56).

#### assetDir  <a href="#assetdir" id="assetdir"></a>

**Default value:** “assets”

The directory where Hugo finds asset files used in [Hugo Pipes](https://gohugo.io/hugo-pipes/). Also see [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts) for an alternative way to configure this directory (from Hugo 0.56).

#### baseURL  <a href="#baseurl" id="baseurl"></a>

The absolute URL (protocol, host, path, and trailing slash) of your published site (e.g., `https://www.example.org/docs/`).

#### build  <a href="#build" id="build"></a>

See [Configure Build](https://gohugo.io/getting-started/configuration/#configure-build)

#### buildDrafts (false)  <a href="#builddrafts-false" id="builddrafts-false"></a>

**Default value:** false

Include drafts when building.

#### buildExpired  <a href="#buildexpired" id="buildexpired"></a>

**Default value:** false

Include content already expired.

#### buildFuture  <a href="#buildfuture" id="buildfuture"></a>

**Default value:** false

Include content with publishdate in the future.

#### caches  <a href="#caches" id="caches"></a>

See [Configure File Caches](https://gohugo.io/getting-started/configuration/#configure-file-caches)

#### cascade  <a href="#cascade" id="cascade"></a>

Pass down default configuration values (front matter) to pages in the content tree. The options in site configuration is the same as in page front matter, see [Front Matter Cascade](https://gohugo.io/content-management/front-matter#front-matter-cascade).

#### canonifyURLs  <a href="#canonifyurls" id="canonifyurls"></a>

**Default value:** false

Enable to turn relative URLs into absolute. See [details](https://gohugo.io/content-management/urls/#canonical-urls).

#### cleanDestinationDir  <a href="#cleandestinationdir" id="cleandestinationdir"></a>

**Default value:** false

When building, removes files from destination not found in static directories.

#### contentDir  <a href="#contentdir" id="contentdir"></a>

**Default value:** “content”

The directory from where Hugo reads content files. Also see [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts) for an alternative way to configure this directory (from Hugo 0.56).

#### copyright  <a href="#copyright" id="copyright"></a>

**Default value:** ""

Copyright notice for your site, typically displayed in the footer.

#### dataDir  <a href="#datadir" id="datadir"></a>

**Default value:** “data”

The directory from where Hugo reads data files. Also see [Module Mounts Config](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts) for an alternative way to configure this directory (from Hugo 0.56).

#### defaultContentLanguage  <a href="#defaultcontentlanguage" id="defaultcontentlanguage"></a>

**Default value:** “en”

Content without language indicator will default to this language.

#### defaultContentLanguageInSubdir  <a href="#defaultcontentlanguageinsubdir" id="defaultcontentlanguageinsubdir"></a>

**Default value:** false

Render the default content language in subdir, e.g. `content/en/`. The site root `/` will then redirect to `/en/`.

#### disableAliases  <a href="#disablealiases" id="disablealiases"></a>

**Default value:** false

Will disable generation of alias redirects. Note that even if `disableAliases` is set, the aliases themselves are preserved on the page. The motivation with this is to be able to generate 301 redirects in an `.htaccess`, a Netlify `_redirects` file or similar using a custom output format.

#### disableHugoGeneratorInject  <a href="#disablehugogeneratorinject" id="disablehugogeneratorinject"></a>

**Default value:** false

Hugo will, by default, inject a generator meta tag in the HTML head on the _home page only_. You can turn it off, but we would really appreciate if you don’t, as this is a good way to watch Hugo’s popularity on the rise.

#### disableKinds  <a href="#disablekinds" id="disablekinds"></a>

**Default value:** \[]

Enable disabling of all pages of the specified _Kinds_. Allowed values in this list: `"page"`, `"home"`, `"section"`, `"taxonomy"`, `"term"`, `"RSS"`, `"sitemap"`, `"robotsTXT"`, `"404"`.

#### disableLiveReload  <a href="#disablelivereload" id="disablelivereload"></a>

**Default value:** false

Disable automatic live reloading of browser window.

#### disablePathToLower  <a href="#disablepathtolower" id="disablepathtolower"></a>

**Default value:** false

Do not convert the url/path to lowercase.

#### enableEmoji  <a href="#enableemoji" id="enableemoji"></a>

**Default value:** false

Enable Emoji emoticons support for page content; see the [Emoji Cheat Sheet](https://www.webpagefx.com/tools/emoji-cheat-sheet/).

#### enableGitInfo  <a href="#enablegitinfo" id="enablegitinfo"></a>

**Default value:** false

Enable `.GitInfo` object for each page (if the Hugo site is versioned by Git). This will then update the `Lastmod` parameter for each page using the last git commit date for that content file.

#### enableInlineShortcodes  <a href="#enableinlineshortcodes" id="enableinlineshortcodes"></a>

**Default value:** false

Enable inline shortcode support. See [Inline Shortcodes](https://gohugo.io/templates/shortcode-templates/#inline-shortcodes).

#### enableMissingTranslationPlaceholders  <a href="#enablemissingtranslationplaceholders" id="enablemissingtranslationplaceholders"></a>

**Default value:** false

Show a placeholder instead of the default value or an empty string if a translation is missing.

#### enableRobotsTXT  <a href="#enablerobotstxt" id="enablerobotstxt"></a>

**Default value:** false

Enable generation of `robots.txt` file.

#### frontmatter  <a href="#frontmatter" id="frontmatter"></a>

See [Front matter Configuration](https://gohugo.io/getting-started/configuration/#configure-front-matter).

#### googleAnalytics  <a href="#googleanalytics" id="googleanalytics"></a>

**Default value:** ""

Google Analytics tracking ID.

#### hasCJKLanguage  <a href="#hascjklanguage" id="hascjklanguage"></a>

**Default value:** false

If true, auto-detect Chinese/Japanese/Korean Languages in the content. This will make `.Summary` and `.WordCount` behave correctly for CJK languages.

#### imaging  <a href="#imaging" id="imaging"></a>

See [image processing configuration](https://gohugo.io/content-management/image-processing/#imaging-configuration).

#### languageCode  <a href="#languagecode" id="languagecode"></a>

**Default value:** ""

A language tag as defined by [RFC 5646](https://datatracker.ietf.org/doc/html/rfc5646). This value is used to populate:

* The `<language>` element in the internal [RSS template](https://github.com/gohugoio/hugo/blob/master/tpl/tplimpl/embedded/templates/\_default/rss.xml)
* The `lang` attribute of the `<html>` element in the internal [alias template](https://github.com/gohugoio/hugo/blob/master/tpl/tplimpl/embedded/templates/alias.html)

#### languages  <a href="#languages" id="languages"></a>

See [Configure Languages](https://gohugo.io/content-management/multilingual/#configure-languages).

#### disableLanguages  <a href="#disablelanguages" id="disablelanguages"></a>

See [Disable a Language](https://gohugo.io/content-management/multilingual/#disable-a-language)

#### markup  <a href="#markup" id="markup"></a>

See [Configure Markup](https://gohugo.io/getting-started/configuration-markup).

#### mediaTypes  <a href="#mediatypes" id="mediatypes"></a>

See [Configure Media Types](https://gohugo.io/templates/output-formats/#media-types).

#### menus  <a href="#menus" id="menus"></a>

See [Menus](https://gohugo.io/content-management/menus/#define-in-site-configuration).

#### minify  <a href="#minify" id="minify"></a>

See [Configure Minify](https://gohugo.io/getting-started/configuration/#configure-minify)

#### module  <a href="#module" id="module"></a>

Module configuration see [module configuration](https://gohugo.io/hugo-modules/configuration/).

#### newContentEditor  <a href="#newcontenteditor" id="newcontenteditor"></a>

**Default value:** ""

The editor to use when creating new content.

#### noChmod  <a href="#nochmod" id="nochmod"></a>

**Default value:** false

Don’t sync permission mode of files.

#### noTimes  <a href="#notimes" id="notimes"></a>

**Default value:** false

Don’t sync modification time of files.

#### outputFormats  <a href="#outputformats" id="outputformats"></a>

See [Configure Output Formats](https://gohugo.io/getting-started/configuration/#configure-additional-output-formats).

#### paginate  <a href="#paginate" id="paginate"></a>

**Default value:** 10

Default number of elements per page in [pagination](https://gohugo.io/templates/pagination/).

#### paginatePath  <a href="#paginatepath" id="paginatepath"></a>

**Default value:** “page”

The path element used during pagination (`https://example.com/page/2`).

#### permalinks  <a href="#permalinks" id="permalinks"></a>

See [Content Management](https://gohugo.io/content-management/urls/#permalinks).

#### pluralizeListTitles  <a href="#pluralizelisttitles" id="pluralizelisttitles"></a>

**Default value:** true

Pluralize titles in lists.

#### publishDir  <a href="#publishdir" id="publishdir"></a>

**Default value:** “public”

The directory to where Hugo will write the final static site (the HTML files etc.).

#### related  <a href="#related" id="related"></a>

See [Related Content](https://gohugo.io/content-management/related/#configure-related-content).

#### relativeURLs  <a href="#relativeurls" id="relativeurls"></a>

**Default value:** false

Enable this to make all relative URLs relative to content root. Note that this does not affect absolute URLs. See [details](https://gohugo.io/content-management/urls/#relative-urls).

#### refLinksErrorLevel  <a href="#reflinkserrorlevel" id="reflinkserrorlevel"></a>

**Default value:** “ERROR”

When using `ref` or `relref` to resolve page links and a link cannot be resolved, it will be logged with this log level. Valid values are `ERROR` (default) or `WARNING`. Any `ERROR` will fail the build (`exit -1`).

#### refLinksNotFoundURL  <a href="#reflinksnotfoundurl" id="reflinksnotfoundurl"></a>

URL to be used as a placeholder when a page reference cannot be found in `ref` or `relref`. Is used as-is.

#### removePathAccents  <a href="#removepathaccents" id="removepathaccents"></a>

**Default value:** false

Removes [non-spacing marks](https://www.compart.com/en/unicode/category/Mn) from [composite characters](https://en.wikipedia.org/wiki/Precomposed\_character) in content paths.

```
content/post/hügó.md → https://example.org/post/hugo/
```

#### rssLimit  <a href="#rsslimit" id="rsslimit"></a>

**Default value:** -1 (unlimited)

Maximum number of items in the RSS feed.

#### sectionPagesMenu  <a href="#sectionpagesmenu" id="sectionpagesmenu"></a>

See [Menus](https://gohugo.io/content-management/menus/#define-automatically).

#### security  <a href="#security" id="security"></a>

See [Security Policy](https://gohugo.io/about/security-model/#security-policy)

#### sitemap  <a href="#sitemap" id="sitemap"></a>

Default [sitemap configuration](https://gohugo.io/templates/sitemap-template/#configuration).

#### summaryLength  <a href="#summarylength" id="summarylength"></a>

**Default value:** 70

The length of text in words to show in a [`.Summary`](https://gohugo.io/content-management/summaries/#automatic-summary-splitting).

#### taxonomies  <a href="#taxonomies" id="taxonomies"></a>

See [Configure Taxonomies](https://gohugo.io/content-management/taxonomies#configure-taxonomies).

#### theme  <a href="#theme" id="theme"></a>

: See [module configuration](https://gohugo.io/hugo-modules/configuration/#module-configuration-imports) for how to import a theme.

#### themesDir  <a href="#themesdir" id="themesdir"></a>

**Default value:** “themes”

The directory where Hugo reads the themes from.

#### timeout  <a href="#timeout" id="timeout"></a>

**Default value:** “30s”

Timeout for generating page contents, specified as a [duration](https://pkg.go.dev/time#Duration) or in seconds. _Note:_ this is used to bail out of recursive content generation. You might need to raise this limit if your pages are slow to generate (e.g., because they require large image processing or depend on remote contents).

#### timeZone  <a href="#timezone" id="timezone"></a>

The time zone (or location), e.g. `Europe/Oslo`, used to parse front matter dates without such information and in the [`time`](https://gohugo.io/functions/time/astime)function. The list of valid values may be system dependent, but should include `UTC`, `Local`, and any location in the [IANA Time Zone database](https://en.wikipedia.org/wiki/List\_of\_tz\_database\_time\_zones).

#### title  <a href="#title" id="title"></a>

Site title.

#### titleCaseStyle  <a href="#titlecasestyle" id="titlecasestyle"></a>

**Default value:** “ap”

See [Configure Title Case](https://gohugo.io/getting-started/configuration/#configure-title-case)

#### uglyURLs  <a href="#uglyurls" id="uglyurls"></a>

**Default value:** false

When enabled, creates URL of the form `/filename.html`instead of `/filename/`.

#### watch  <a href="#watch" id="watch"></a>

**Default value:** false

Watch filesystem for changes and recreate as needed.

If you are developing your site on a \*nix machine, here is a handy shortcut for finding a configuration option from the command line:

```txt
cd ~/sites/yourhugosite
hugo config | grep emoji
```

which shows output like

```txt
enableemoji: true
```

### Configure build  <a href="#configure-build" id="configure-build"></a>

The `build` configuration section contains global build-related configuration options.

hugo.yaml toml json&#x20;

```toml
[build]
  noJSConfigInAssets = false
  useResourceCacheWhen = 'fallback'
  [build.buildStats]
    disableClasses = false
    disableIDs = false
    disableTags = false
    enable = false
[[build.cacheBusters]]
    source = 'assets/.*\.(js|ts|jsx|tsx)'
    target = '(js|scripts|javascript)'
[[build.cacheBusters]]
    source = 'assets/.*\.(css|sass|scss)$'
    target = '(css|styles|scss|sass)'
[[build.cacheBusters]]
    source = '(postcss|tailwind)\.config\.js'
    target = '(css|styles|scss|sass)'
[[build.cacheBusters]]
    source = 'assets/.*\.(.*)$'
    target = '$1'
```

buildStats [New in v0.115.1](https://github.com/gohugoio/hugo/releases/tag/v0.115.1)When enabled, creates a `hugo_stats.json` file in the root of your project. This file contains arrays of the `class` attributes, `id`attributes, and tags of every HTML element within your published site. Use this file as data source when [removing unused CSS](https://gohugo.io/hugo-pipes/postprocess/#css-purging-with-postcss) from your site. This process is also known as pruning, purging, or tree shaking.

Exclude `class` attributes, `id` attributes, or tags from `hugo_stats.json` with the `disableClasses`, `disableIDs`, and `disableTags` keys.

With v0.115.0 and earlier this feature was enabled by setting `writeStats` to `true`. Although still functional, the `writeStats` key will be deprecated in a future release.

Given that CSS purging is typically limited to production builds, place the `buildStats` object below [config/production](https://gohugo.io/getting-started/configuration/#configuration-directory).

Built for speed, there may be “false positive” detections (e.g., HTML elements that are not HTML elements) while parsing the published site. These “false positives” are infrequent and inconsequential.

Due to the nature of partial server builds, new HTML entities are added while the server is running, but old values will not be removed until you restart the server or run a regular `hugo` build.

cachebustersSee [Configure Cache Busters](https://gohugo.io/getting-started/configuration/#configure-cache-busters)noJSConfigInAssetsTurn off writing a `jsconfig.json` into your `/assets` folder with mapping of imports from running [js.Build](https://gohugo.io/hugo-pipes/js). This file is intended to help with intellisense/navigation inside code editors such as [VS Code](https://code.visualstudio.com/). Note that if you do not use `js.Build`, no file will be written.useResourceCacheWhenWhen to use the cached resources in `/resources/_gen` for PostCSS and ToCSS. Valid values are `never`, `always` and `fallback`. The last value means that the cache will be tried if PostCSS/extended version is not available.

### Configure cache busters  <a href="#configure-cache-busters" id="configure-cache-busters"></a>

[New in v0.112.0](https://github.com/gohugoio/hugo/releases/tag/v0.112.0)

The `build.cachebusters` configuration option was added to support development using Tailwind 3.x’s JIT compiler where a `build` configuration may look like this:

hugo.yaml toml json&#x20;

```toml
[build]
  [build.buildStats]
    enable = true
[[build.cachebusters]]
    source = 'assets/watching/hugo_stats\.json'
    target = 'styles\.css'
[[build.cachebusters]]
    source = '(postcss|tailwind)\.config\.js'
    target = 'css'
[[build.cachebusters]]
    source = 'assets/.*\.(js|ts|jsx|tsx)'
    target = 'js'
[[build.cachebusters]]
    source = 'assets/.*\.(.*)$'
    target = '$1'
```

Some key points in the above are `writeStats = true`, which writes a `hugo_stats.json` file on each build with HTML classes etc. that’s used in the rendered output. Changes to this file will trigger a rebuild of the `styles.css` file. You also need to add `hugo_stats.json` to Hugo’s server watcher. See [Hugo Starter Tailwind Basic](https://github.com/bep/hugo-starter-tailwind-basic) for a running example.

sourceA regexp matching file(s) relative to one of the virtual component directories in Hugo, typically `assets/...`.targetA regexp matching the keys in the resource cache that should be expired when `source` changes. You can use the matching regexp groups from `source` in the expression, e.g. `$1`.

### Configure server  <a href="#configure-server" id="configure-server"></a>

This is only relevant when running `hugo server`, and it allows to set HTTP headers during development, which allows you to test out your Content Security Policy and similar. The configuration format matches [Netlify’s](https://docs.netlify.com/routing/headers/#syntax-for-the-netlify-configuration-file) with slightly more powerful [Glob matching](https://github.com/gobwas/glob):

hugo.yaml toml json&#x20;

```toml
[server]
[[server.headers]]
    for = '/**'
    [server.headers.values]
      Content-Security-Policy = 'script-src localhost:1313'
      Referrer-Policy = 'strict-origin-when-cross-origin'
      X-Content-Type-Options = 'nosniff'
      X-Frame-Options = 'DENY'
      X-XSS-Protection = '1; mode=block'
```

Since this is “development only”, it may make sense to put it below the `development` environment:

config/development/server.yaml toml json&#x20;

```toml
[[headers]]
  for = '/**'
  [headers.values]
    Content-Security-Policy = 'script-src localhost:1313'
    Referrer-Policy = 'strict-origin-when-cross-origin'
    X-Content-Type-Options = 'nosniff'
    X-Frame-Options = 'DENY'
    X-XSS-Protection = '1; mode=block'
```

You can also specify simple redirects rules for the server. The syntax is again similar to Netlify’s.

Note that a `status` code of 200 will trigger a [URL rewrite](https://docs.netlify.com/routing/redirects/rewrites-proxies/), which is what you want in SPA situations, e.g:

config/development/server.yaml toml json&#x20;

```toml
[[redirects]]
  force = false
  from = '/myspa/**'
  status = 200
  to = '/myspa/'
```

Setting `force=true` will make a redirect even if there is existing content in the path. Note that before Hugo 0.76 `force` was the default behavior, but this is inline with how Netlify does it.

### 404 server error page  <a href="#_404-server-error-page" id="_404-server-error-page"></a>

[New in v0.103.0](https://github.com/gohugoio/hugo/releases/tag/v0.103.0)

Hugo will, by default, render all 404 errors when running `hugo server` with the `404.html` template. Note that if you have already added one or more redirects to your [server configuration](https://gohugo.io/getting-started/configuration/#configure-server), you need to add the 404 redirect explicitly, e.g:

config/development/server.yaml toml json&#x20;

```toml
[[redirects]]
  from = '/**'
  status = 404
  to = '/404.html'
```

### Configure title case  <a href="#configure-title-case" id="configure-title-case"></a>

Set `titleCaseStyle` to specify the title style used by the [title](https://gohugo.io/functions/strings/title)template function and the automatic section titles in Hugo.

Can be one of:

* `ap` (default), the capitalization rules in the [Associated Press (AP) Stylebook](https://www.apstylebook.com/)
* `chicago`, the [Chicago Manual of Style](https://www.chicagomanualofstyle.org/home.html)
* `go`, Go’s convention of capitalizing every word.
* `firstupper`, capitalize the first letter of the first word.
* `none`, no capitalization.

### Configuration environment variables  <a href="#configuration-environment-variables" id="configuration-environment-variables"></a>

HUGO\_NUMWORKERMULTIPLIERCan be set to increase or reduce the number of workers used in parallel processing in Hugo. If not set, the number of logical CPUs will be used.

### Configuration lookup order  <a href="#configuration-lookup-order" id="configuration-lookup-order"></a>

Similar to the template [lookup order](https://gohugo.io/templates/lookup-order/), Hugo has a default set of rules for searching for a configuration file in the root of your website’s source directory as a default behavior:

1. `./hugo.toml`
2. `./hugo.yaml`
3. `./hugo.json`

In your configuration file, you can direct Hugo as to how you want your website rendered, control your website’s menus, and arbitrarily define site-wide parameters specific to your project.

### Example configuration  <a href="#example-configuration" id="example-configuration"></a>

The following is a typical example of a configuration file. The values nested under `params:` will populate the [`.Site.Params`](https://gohugo.io/variables/site/)variable for use in [templates](https://gohugo.io/templates/):

hugo.yaml toml json&#x20;

```toml
baseURL = 'https://yoursite.example.com/'
title = 'My Hugo Site'
[params]
  AuthorName = 'Jon Doe'
  GitHubUser = 'spf13'
  ListOfFoo = ['foo1', 'foo2']
  SidebarRecentLimit = 5
  Subtitle = 'Hugo is Absurdly Fast!'
[permalinks]
  posts = '/:year/:month/:title/'
```

### Configure with environment variables  <a href="#configure-with-environment-variables" id="configure-with-environment-variables"></a>

In addition to the 3 configuration options already mentioned, configuration key-values can be defined through operating system environment variables.

For example, the following command will effectively set a website’s title on Unix-like systems:

```txt
$ env HUGO_TITLE="Some Title" hugo
```

This is really useful if you use a service such as Netlify to deploy your site. Look at the Hugo docs [Netlify configuration file](https://github.com/gohugoio/hugoDocs/blob/master/netlify.toml) for an example.

Names must be prefixed with `HUGO_` and the configuration key must be set in uppercase when setting operating system environment variables.

To set configuration parameters, prefix the name with `HUGO_PARAMS_`

If you are using snake\_cased variable names, the above will not work. Hugo determines the delimiter to use by the first character after `HUGO`. This allows you to define environment variables on the form `HUGOxPARAMSxAPI_KEY=abcdefgh`, using any [allowed](https://stackoverflow.com/questions/2821043/allowed-characters-in-linux-environment-variable-names) delimiter.

### Ignore content and data files when rendering  <a href="#ignore-content-and-data-files-when-rendering" id="ignore-content-and-data-files-when-rendering"></a>

**Note:** This works, but we recommend you use the newer and more powerful [includeFiles and excludeFiles](https://gohugo.io/hugo-modules/configuration/#module-configuration-mounts) mount options.

To exclude specific files from the `content`, `data`, and `i18n`directories when rendering your site, set `ignoreFiles` to one or more regular expressions to match against the absolute file path.

To ignore files ending with `.foo` or `.boo`:

hugo.yaml toml json&#x20;

```toml
ignoreFiles = ['\.foo$', '\.boo$']
```

To ignore a file using the absolute file path:

hugo.yaml toml json&#x20;

```toml
ignoreFiles = ['^/home/user/project/content/test\.md$']
```

### Configure front matter  <a href="#configure-front-matter" id="configure-front-matter"></a>

#### Configure dates  <a href="#configure-dates" id="configure-dates"></a>

Dates are important in Hugo, and you can configure how Hugo assigns dates to your content pages. You do this by adding a `frontmatter` section to your `hugo.toml`.

The default configuration is:

hugo.yaml toml json&#x20;

```toml
[frontmatter]
  date = ['date', 'publishdate', 'pubdate', 'published', 'lastmod', 'modified']
  expiryDate = ['expirydate', 'unpublishdate']
  lastmod = [':git', 'lastmod', 'modified', 'date', 'publishdate', 'pubdate', 'published']
  publishDate = ['publishdate', 'pubdate', 'published', 'date']
```

If you, as an example, have a non-standard date parameter in some of your content, you can override the setting for `date`:

hugo.yaml toml json&#x20;

```toml
[frontmatter]
  date = ['myDate', ':default']
```

The `:default` is a shortcut to the default settings. The above will set `.Date` to the date value in `myDate` if present, if not we will look in `date`,`publishDate`, `lastmod` and pick the first valid date.

In the list to the right, values starting with “:” are date handlers with a special meaning (see below). The others are just names of date parameters (case insensitive) in your front matter configuration. Also note that Hugo have some built-in aliases to the above: `lastmod` => `modified`, `publishDate` => `pubdate`, `published` and `expiryDate` => `unpublishdate`. With that, as an example, using `pubDate` as a date in front matter, will, by default, be assigned to `.PublishDate`.

The special date handlers are:

`:fileModTime`Fetches the date from the content file’s last modification timestamp.

An example:

hugo.yaml toml json&#x20;

```toml
[frontmatter]
  lastmod = ['lastmod', ':fileModTime', ':default']
```

The above will try first to extract the value for `.Lastmod` starting with the `lastmod` front matter parameter, then the content file’s modification timestamp. The last, `:default` should not be needed here, but Hugo will finally look for a valid date in `:git`, `date` and then `publishDate`.

`:filename`Fetches the date from the content file’s file name. For example, `2018-02-22-mypage.md` will extract the date `2018-02-22`. Also, if `slug` is not set, `mypage` will be used as the value for `.Slug`.

An example:

hugo.yaml toml json&#x20;

```toml
[frontmatter]
  date = [':filename', ':default']
```

The above will try first to extract the value for `.Date` from the file name, then it will look in front matter parameters `date`, `publishDate` and lastly `lastmod`.

`:git`This is the Git author date for the last revision of this content file. This will only be set if `--enableGitInfo` is set or `enableGitInfo = true` is set in site configuration.

### Configure additional output formats  <a href="#configure-additional-output-formats" id="configure-additional-output-formats"></a>

Hugo v0.20 introduced the ability to render your content to multiple output formats (e.g., to JSON, AMP html, or CSV). See [Output Formats](https://gohugo.io/templates/output-formats/) for information on how to add these values to your Hugo project’s configuration file.

### Configure minify  <a href="#configure-minify" id="configure-minify"></a>

Default configuration:

hugo.yaml toml json&#x20;

```toml
[minify]
  disableCSS = false
  disableHTML = false
  disableJS = false
  disableJSON = false
  disableSVG = false
  disableXML = false
  minifyOutput = false
  [minify.tdewolff]
    [minify.tdewolff.css]
      keepCSS2 = true
      precision = 0
    [minify.tdewolff.html]
      keepComments = false
      keepConditionalComments = true
      keepDefaultAttrVals = true
      keepDocumentTags = true
      keepEndTags = true
      keepQuotes = false
      keepWhitespace = false
    [minify.tdewolff.js]
      keepVarNames = false
      precision = 0
      version = 2022
    [minify.tdewolff.json]
      keepNumbers = false
      precision = 0
    [minify.tdewolff.svg]
      keepComments = false
      precision = 0
    [minify.tdewolff.xml]
      keepWhitespace = false
```

### Configure file caches  <a href="#configure-file-caches" id="configure-file-caches"></a>

Since Hugo 0.52 you can configure more than just the `cacheDir`. This is the default configuration:

hugo.yaml toml json&#x20;

```toml
[caches]
  [caches.assets]
    dir = ':resourceDir/_gen'
    maxAge = -1
  [caches.getcsv]
    dir = ':cacheDir/:project'
    maxAge = -1
  [caches.getjson]
    dir = ':cacheDir/:project'
    maxAge = -1
  [caches.getresource]
    dir = ':cacheDir/:project'
    maxAge = -1
  [caches.images]
    dir = ':resourceDir/_gen'
    maxAge = -1
  [caches.modules]
    dir = ':cacheDir/modules'
    maxAge = -1
```

You can override any of these cache settings in your own `hugo.toml`.

#### The keywords explained  <a href="#the-keywords-explained" id="the-keywords-explained"></a>

`:cacheDir`See [Configure cacheDir](https://gohugo.io/getting-started/configuration/#configure-cachedir).`:project`The base directory name of the current Hugo project. This means that, in its default setting, every project will have separated file caches, which means that when you do `hugo --gc` you will not touch files related to other Hugo projects running on the same PC.`:resourceDir`This is the value of the `resourceDir` configuration option.maxAgeThis is the duration before a cache entry will be evicted, -1 means forever and 0 effectively turns that particular cache off. Uses Go’s `time.Duration`, so valid values are `"10s"` (10 seconds), `"10m"` (10 minutes) and `"10h"` (10 hours).dirThe absolute path to where the files for this cache will be stored. Allowed starting placeholders are `:cacheDir` and `:resourceDir` (see above).

### Configuration format specs  <a href="#configuration-format-specs" id="configuration-format-specs"></a>

* [TOML Spec](https://toml.io/en/latest)
* [YAML Spec](https://yaml.org/spec/)
* [JSON Spec](https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)

### Configure cacheDir  <a href="#configure-cachedir" id="configure-cachedir"></a>

This is the directory where Hugo by default will store its file caches. See [Configure File Caches](https://gohugo.io/getting-started/configuration/#configure-file-caches).

This can be set using the `cacheDir` config option or via the OS env variable `HUGO_CACHEDIR`.

If this is not set, Hugo will use, in order of preference:

1. If running on Netlify: `/opt/build/cache/hugo_cache/`. This means that if you run your builds on Netlify, all caches configured with `:cacheDir` will be saved and restored on the next build. For other CI vendors, please read their documentation. For an CircleCI example, see [this configuration](https://github.com/bep/hugo-sass-test/blob/6c3960a8f4b90e8938228688bc49bdcdd6b2d99e/.circleci/config.yml).
2. In a `hugo_cache` directory below the OS user cache directory as defined by Go’s [os.UserCacheDir](https://pkg.go.dev/os#UserCacheDir). On Unix systems, this is `$XDG_CACHE_HOME` as specified by [basedir-spec-latest](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) if non-empty, else `$HOME/.cache`. On MacOS, this is `$HOME/Library/Caches`. On Windows, this is`%LocalAppData%`. On Plan 9, this is `$home/lib/cache`.[New in v0.116.0](https://github.com/gohugoio/hugo/releases/tag/v0.116.0)
3. In a `hugo_cache_$USER` directory below the OS temp dir.

If you want to know the current value of `cacheDir`, you can run `hugo config`, e.g: `hugo config | grep cachedir`.
