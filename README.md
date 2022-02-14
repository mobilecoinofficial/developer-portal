Welcome to the MobileCoin developer documentation! This repository contains the content for the [MobileCoin developer portal](https://developers.mobilecoin.com). If you are looking for the portal, please go there. If you intend to submit a change to the developer docs, please read the below structural guidelines.

# Site Structure

The "Content" folder represents the starting point for all site content. Anything above that in the file structure will be ignored, as will any README files within the site content folders.

The "Overview," "Guides," "Community," and "FAQ" folders just underneath the main content folder map directly to the corresponding sections in the developer portal.

Documents in "Overview," "Guides," and "FAQ" will be constructed into traditional documentation format with a linked table of contents. Files will render into html from the markdown, while directories will be interpreted as subgroup headings.

# Markdown, Directories, and Metadata

## Tables

The Developer Portal uses GFM as its markdown interpreter—anything that would render here in GitHub should render on the site as expected. HTML is also allowed. A note that when making tables, please remember to include the heading dividers and that extra line breaks may cause the table not to appear properly:

| heading | heading |

| ------- | ------- | <-- important!

| data    | data    |

## Alerts

Using the following formatting items will render the line as an information block, a warning block, or an error block:

?> this is helpful information

!> this is a warning

x> this is an error

## Filenames

Pages and subgroups will render *in the order they appear in the directory*. For best results, please add a four-digit number to the start of files and folders, followed by an underscore:

**0010_document.md**

This will ensure they appear in the intended order on the final site.

## Headers and Metadata

All site markdown files should include a YAML-style header which allows The following parameters. This will allow pages to better generate their meta tags, and give some more control over the appearance of things.

```
---
title: "optional. Recommended in quotes to avoid formatting faux-pas. A missing title will use the filename starting after the first _ character. Pages will draw this title as an h1 at the top of the document."
author: "optional. The original author of the page, if desired."
description: "optional. text to be used as the description in meta tags and in internal search previews."
hide_title: optional boolean. When true, doesn't render the title given in this header on the page, for when you want to include it yourself.
---
```

## Images and Other Assets

If you'd like to include an image in one of these markdown documents, the usual method will work:

![alt text](https://mobilecoin.com/images/heart.svg)

You can use a local file by referencing it's root path as it would be in *this repository*. The interpreter will create the proper resource directory on the final site to match this one. That means if it shows up in GitHub, it'll show up on the final site! The below image comes from /images/fog.png :

![start with a slash](/images/fog.png)

Images will try to fill the width of their container by default. If a width is given using an html `<img>` tag, the image will be centered in the document. You can attach classes "left" or "right" to the image to have it align to either side. This is also true of `<video>` and `<iframe>` tags.

# Pulling from External Locations

This site is able to pull in markdown content from other arbitrary repositories as well, provided the document you want to include is publicly visible on GitHub. To do so, just include a markdown file with `use_file: [github path]` line in the header. The site front-end will build a page using the local head data and the provided content as the body.

This system uses the GitHub content API to retrive data, so the path provided must take the common GitHub form of `[owner]/[repository]/[path to file]`

For example, to get the [readme for the MobileCoin Foundation's "Mechanics of MobileCoin" repo](https://github.com/mobilecoinfoundation/Mechanics-of-MobileCoin/blob/master/README.md), use the path 'mobilecoinfoundation/Mechanics-of-MobileCoin/README.md' for the `use_file` field. It is recommended to still include some body content in these markdown files, as it will be used as the fallback content should the site be unable to reach to remote content for any reason.
