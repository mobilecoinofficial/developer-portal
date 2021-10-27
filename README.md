Welcome to the MobileCoin developer documentation! This repository contains the content for the [MobileCoin developer portal](https://developers.mobilecoin.com). If you are looking for the portal, please go there. If you intend to submit a change to the developer docs, please read the below structural guidelines.

# Site Structure

The "Content" folder represents the starting point for all site content. Anything above that in the file structure will be ignored, as will any README files within the site content folders.

The "Overview," "How-to Guides," "Community," and "FAQ" folders just underneath the main content folder map directly to the corresponding sections in the developer portal.

Documents in the first two, "Overview" and "How-to Guides," will be constructed into traditional documentation format with a linked table of contents. Files will render into html from the markdown, while directories will be interpreted as subgroup headings. Any H1 (#) or H2 (##) elements will also be rendered to the table to the table of contents for quick access.

The "Community" and "FAQ" sections are still in the design stage.

# Markdown, Directories, and Metadata

Pages and subgroups will render *in the order they appear in the directory*. For best results, please add a four-digit number to the start of files and folders, followed by an underscore:

**0010_document.md**

This will ensure they appear in the intended order on the final site.

All site markdown files should include a YAML-style header with the following data:
```
---
title: "not required. Recommended in quotes to avoid formatting faux-pas. A missing title will use the filename starting after the first _ character"
author: "not required. The original author of the page."
description: "text to be used as the description in meta tags."
---
```
