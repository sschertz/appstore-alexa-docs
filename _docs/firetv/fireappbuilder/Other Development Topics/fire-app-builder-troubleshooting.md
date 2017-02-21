---
title: Troubleshooting
permalink: fire-app-builder-troubleshooting.html
sidebar: fireappbuilder
product: Fire App Builder
toc: false
github: true
---

First consult the [Known Issues][fire-app-builder-release-notes#known_issues] section to see if any bugs you're experiencing are known. 

If you can't resolve your issue, see the [Fire TV and Fire TV Stick](https://forums.developer.amazon.com/spaces/43/Fire+TV+and+Fire+TV+Stick.html) categories on the Support Forums.

## Build Errors

**Problem**: You try to build the project but get an error that says:

```
Error: Content is not allowed in prolog
```

This error is related to Windows. When you cloned the Github repo, git didn't have symbolic linking configured to `true`. As a result, the symlinks used for some of the XML files didn't copy down with the right content.

**Resolution**: Configure git to use symbolic links:

```
git config –global core.symlinks true 
```

Then re-clone the repo and build the project again. You can verify that symlinks are working by looking at the strings.xml file in **Utils > src > main > res > values > strings.xml > strings.xml (en-rUS)**. If you see normal content, symlinks are working. In contrast, if there's just a short reference and nothing else, symlinks aren't working.

{% include links.html %}
