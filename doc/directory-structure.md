# Directory structure

```
.
└── /lib/tpl/jeece
    ├── css                        # CSS and LESS files; Originals from the DokuWiki default template, specific
    |   ├── ...                    # modifications of the existant are done there but new files are in the less folder
    |   └── _app.css               # output of the custom LESS files
    ├── less                       # custom LESS files, compiled before being handled by DokuWiki (DokuWiki doesn't
    |   |                          # support the raw Twitter Bootstrap LESS files so we must compile them beforehand)
    |   ├── components             # components style
    |   ├── core                   # configuration file and variables
    |   ├── libs                   # external libraries
    |   ├── utilities              # helpers and cie
    |   └── app.less               # main LESS file, is compiled into css/_app.css
    ├── js                         # Javascript files
    |   ├── libs                   # external libraries
    |   ├── src                    # Javascript files
    |   └── app.min.js             # output Javascript file
    ├── script.js                  # main Javascript file; onload and onresize defined there
    ├── images                     # all images used in the template
    |
    ├── pages                      # HTML files 
    |   ├── footer.html            # at the very end of the page just before the closing body tag
    |   ├── header.html            # below the pagename and wiki title
    |   ├── mata.html              # custom meta data
    |   ├── page.html              # page content layout
    |   ├── page_actions.html      # page actions (pagetools)
    |   ├── pagefooter.html        # below the last change date
    |   ├── pageheader.html        # below the breadcrumbs and above the actual content
    |   ├── sidebar.html           # sidebar layout
    |   ├── top_fixed_navbar.html  # top part of the header
    |   └── top_navbar.html        # second part of the header
    ├── topheader.html             # file included right after the body tag
    ├── detail.php                 # inheritance of the DokuWiki default template; check its use and get over with it
    ├── main.php                   # general layout of DokuWiki
    ├── mediamanager.php           # the media selection popup //TODO: update it
    |
    ├── style.ini                  # specify the CSS files and some global variables; should at least specify one print
    |                              # and one scree CSS file
    ├── template.info.txt          # a text file with template information
    |
    ├── bower.json                 # Bower package configuration file; used only for dependency management
    ├── package.json               # NPM package configuration file; used only for Grunt dependency management
    └── Gruntfile.js               # Grunt tasks configuration file

```

## Global informations

### CSS loading

DokuWiki load it's stylesheets from `lib/exe/css.php` which handles caching, pattern replacement, LESS preprocessing and optimizing. The loading is done via the [`tpl_metaheaders()`](http://xref.dokuwiki.org/reference/dokuwiki/nav.html?_functions/tpl_metaheaders.html) function.

Complete documentation [here](//www.dokuwiki.org/devel:css).


### Stylesheet modes

The mode of a stylesheet is defined in the [`style.ini`](../style.ini) file. The available modes are:

* **screen** : this is used when displaying pages in the web browser
* **print** : definitions here will be used for printing pages
* **all** : applied in all display modes


#### DokuWiki stylesheets

DokuWiki loads its stylesheets from 4 sources, which are loaded in this order:

* Base stylesheets located in `/lib/styles`: they define basic styling, like the appearance of error messages.
* Plugins styles
* Templates styles: given by the [`style.ini`](../style.ini) file
* User styles


#### Using IDs

To use CSS IDs in the template, we add two adjacents underscores `__`. Indeed the IDs automatically assigned are always valid pagenames so cannot contain this pattern. This ensure that there will be no conflicts in the ID's set manually and the ones generated automatically by Dokuwiki.


#### Using images and importing styles

Relative links to images (`url(...)`) and imported stylesheets (`@import`) are automatically fixed by DokuWiki's CSS dispatcher by treating them relative to the template's root directory.

So to include the image `img1.png` which is in the folder `images`, use the path `images/img1.png`.


### Javascript loading

All JavaScript is collected and delivered by `lib/exe/js.php`. This file will concatenate all found files, whitespace compress (if compress is enabled) and cache the result. It also instructs browsers to cache the file, so when you are developing new JavaScript, be sure to refresh your browser cache (hitting `Shift`+`F5`, `Shift`+`CTL`+`R or similar) whenever your script was updated.

DokuWiki will load JavaScript from the following places:

* autogenerated JavaScript (language strings, config settings, toolbar)
* `lib/scripts/*.js`
* `lib/plugins/*/script.js`
* `lib/tpl/<currenttemplate>/script.js`
* `conf/userscript.js`

Full documentation [here](https://www.dokuwiki.org/devel:javascript).


### About the main.php file

Full documentation [here](//www.dokuwiki.org/devel:templates:main.php).


### Avoiding problems

Here are a few problems template developers run into and how to avoid them:

* Don't put JavaScript commands in the `<body>` tag of a page. This includes onLoad and others. Although breaking this rule doesn't affect FireFox at all, Internet Explorer (even IE 7) will have JavaScript errors due to the JavaScript required for page editing, and this can result in pages that won't display correctly, and you will find the editing bar will be missing when you need it.
* Turn off `Compact CSS and JavaScript files` while developing a template.
* Some template developers experience problems with DokuWiki cacheing CSS and JS files due to this option being on, although this has been hard to pinpoint. To be safe, turn this off temporarily.
* Use `forced refreshing` after you make changes to CSS files. You can accomplish this by pressing `Ctrl+F5` or by holding down shift-control-alt and hitting the refresh button in your browser.


### Template style.ini

Full documentation [here](//www.dokuwiki.org/devel:style.ini).

The [`style.ini`](../style.ini) is a ini file in each template's directory configuring the generation of CSS styles. It has two sections: `[stylesheets]` and `[replacements]` described below.

#### [stylesheets]

This part defines which CSS files are loaded. Each line is defined as follow:

```bash
cssfile.css = mode
```

`cssfile.css` is your file name and mode is one of the supported output modes as described at mode. The stylesheet can also be a `.less file`.



#### [replacements]

DokuWiki's CSS dispatcher is able to replace placeholders in the loaded stylesheets which are configured through the `[replacements]` section in the templates [`style.ini`](../style.ini). This is especially useful for defining a color scheme once and reuse these colors in all files. These replacements can also be used as LESS variables.


## Template information

Full documentation available [here](//www.dokuwiki.org/devel:template_info).

[Previous section](dependencies-install.md)<br />
[Next section](getting-started.md)
