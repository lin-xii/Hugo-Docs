# Quick start

Learn to create a Hugo site in minutes.

In this tutorial you will:

1. Create a site
2. Add content
3. Configure the site
4. Publish the site

### Prerequisites  <a href="#prerequisites" id="prerequisites"></a>

Before you begin this tutorial you must:

1. [Install Hugo](https://gohugo.io/installation/) (extended edition, v0.112.0 or later)
2. [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

You must also be comfortable working from the command line.

### Create a site  <a href="#create-a-site" id="create-a-site"></a>

#### Commands  <a href="#commands" id="commands"></a>

**If you are a Windows user:**

* Do not use the Command Prompt
* Do not use Windows PowerShell
* Run these commands from [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows) or a Linux terminal such as WSL or Git Bash

PowerShell and Windows PowerShell [are different applications](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.3).

Run these commands to create a Hugo site with the [Ananke](https://github.com/theNewDynamic/gohugo-theme-ananke)theme. The next section provides an explanation of each command.

```
hugo new site quickstart
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
echo "theme = 'ananke'" >> hugo.toml
hugo server
```

View your site at the URL displayed in your terminal. Press `Ctrl + C` to stop Hugo’s development server.

#### Explanation of commands  <a href="#explanation-of-commands" id="explanation-of-commands"></a>

Create the [directory structure](https://gohugo.io/getting-started/directory-structure) for your project in the `quickstart`directory.

```
hugo new site quickstart
```

Change the current directory to the root of your project.

```
cd quickstart
```

Initialize an empty Git repository in the current directory.

```
git init
```

Clone the [Ananke](https://github.com/theNewDynamic/gohugo-theme-ananke) theme into the `themes` directory, adding it to your project as a [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

```
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

Append a line to the site configuration file, indicating the current theme.

```
echo "theme = 'ananke'" >> hugo.toml
```

Start Hugo’s development server to view the site.

```
hugo server
```

Press `Ctrl + C` to stop Hugo’s development server.

### Add content  <a href="#add-content" id="add-content"></a>

Add a new page to your site.

```
hugo new content posts/my-first-post.md
```

Hugo created the file in the `content/posts` directory. Open the file with your editor.

```
---
title: "My First Post"
date: 2022-11-20T09:03:20-08:00
draft: true
---
```

Notice the `draft` value in the [front matter](https://gohugo.io/content-management/front-matter) is `true`. By default, Hugo does not publish draft content when you build the site. Learn more about [draft, future, and expired content](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content).

Add some [markdown](https://commonmark.org/help/) to the body of the post, but do not change the `draft` value.

```
---
title: "My First Post"
date: 2022-11-20T09:03:20-08:00
draft: true
---
## Introduction

This is **bold** text, and this is *emphasized* text.

Visit the [Hugo](https://gohugo.io) website!
```

Save the file, then start Hugo’s development server to view the site. You can run either of the following commands to include draft content.

```
hugo server --buildDrafts
hugo server -D
```

View your site at the URL displayed in your terminal. Keep the development server running as you continue to add and change content.

Hugo’s rendering engine conforms to the CommonMark [specification](https://spec.commonmark.org/) for markdown. The CommonMark organization provides a useful [live testing tool](https://spec.commonmark.org/dingus/) powered by the reference implementation.

### Configure the site  <a href="#configure-the-site" id="configure-the-site"></a>

With your editor, open the [site configuration](https://gohugo.io/getting-started/configuration/) file (`hugo.toml`) in the root of your project.

```
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'ananke'
```

Make the following changes:

1. Set the `baseURL` for your production site. This value must begin with the protocol and end with a slash, as shown above.
2. Set the `languageCode` to your language and region.
3. Set the `title` for your production site.

Start Hugo’s development server to see your changes, remembering to include draft content.

```
hugo server -D
```

Most theme authors provide configuration guidelines and options. Make sure to visit your theme’s repository or documentation site for details.

[The New Dynamic](https://www.thenewdynamic.com/), authors of the Ananke theme, provide [documentation](https://github.com/theNewDynamic/gohugo-theme-ananke#readme) for configuration and usage. They also provide a [demonstration site](https://gohugo-ananke-theme-demo.netlify.app/).

### Publish the site  <a href="#publish-the-site" id="publish-the-site"></a>

In this step you will _publish_ your site, but you will not _deploy_ it.

When you _publish_ your site, Hugo creates the entire static site in the `public` directory in the root of your project. This includes the HTML files, and assets such as images, CSS files, and JavaScript files.

When you publish your site, you typically do _not_ want to include [draft, future, or expired content](https://gohugo.io/getting-started/usage/#draft-future-and-expired-content). The command is simple.

```
hugo
```

To learn how to _deploy_ your site, see the [hosting and deployment](https://gohugo.io/hosting-and-deployment/) section.

### Ask for help  <a href="#ask-for-help" id="ask-for-help"></a>

Hugo’s [forum](https://discourse.gohugo.io/) is an active community of users and developers who answer questions, share knowledge, and provide examples. A quick search of over 20,000 topics will often answer your question. Please be sure to read about [requesting help](https://discourse.gohugo.io/t/requesting-help/9132) before asking your first question.

### Other resources  <a href="#other-resources" id="other-resources"></a>

For other resources to help you learn Hugo, including books and video tutorials, see the [external learning resources](https://gohugo.io/getting-started/external-learning-resources/) page.
