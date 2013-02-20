---
layout: post
title: "How to Use the Stately WebFont in a Rails Application"
date: 2013-02-20 12:32
comments: true
categories: [Ruby on Rails]
---

A few weeks ago, [Ben Markowitz](https://twitter.com/bpmarkowitz), a UX designer at [Intridea](http://www.intridea.com/), released an open source web font called [Stately](http://intridea.github.com/stately/). Stately allows designers to make simple, great-looking U.S. map visualizations using only HTML and CSS. For example:

{% img /images/posts/stately_example.jpg [Example of a map created using Stately] %}

Using Stately in a Rails application is pretty straightforward, but it does require a little configuration to get it to work with the Rails asset pipeline.

<!-- more -->

**Install Stately in Your Rails Application**

First, [download Stately](https://github.com/intridea/stately) from Github. The package includes several files, but the ones to focus on for Rails installation are in two directories:

+ `stately-master/assets/font` directory containing four web font files
+ `stately-master/assets/sass` directory containing four SASS files that handle the basic Stately CSS configuration, and allow you to customize the styles (i.e. assigning different colors to specific states).

Copy the .scss files from the `stately-master/assets/sass` directory into the `vendor/assets/stylesheets` directory of your Rails application.

Next, create a new directory in the vendor/assets folder to store your fonts, such as `vendor/assets/fonts`. Copy the four font files from `stately-master/assets/font` into this new fonts direcetory.

**Configure the Rails Asset Pipeline to Recognize Stately**

In Rails 3.1+, the asset pipeline helps streamline the inclusion of stylesheets, javascript, images and other assets (like fonts!) for use in your application. Rails manages the inclusion of these assets using manifest files, like the one included in `app/assets/stylesheets/application.css`. You need to edit this manifest to include Stately:

``` ruby Add Stately to the manifest file (app/assets/stylesheets/application.css)
# ...
*= require_self
*= require_tree .
*= require stately    # add stately to the manifest
*/
# ...
```

This will tell Rails to load the `stately.scss` file in your vendor directory, which in turn loads Stately's `_setup.scss` and `_customizations.scss` stylesheets, giving you access to the Stately CSS classes described [in the documentation](https://github.com/intridea/stately).

However, the default `@font-face` settings in Stately's `_setup.scss` file don't work with the Rails pipeline, because the paths it uses to load the fonts will no longer be correct. In fact, the asset pipeline makes the paths simpler. Edit Stately's `_setup.scss` stylesheet so that its `@font-face` `src:` commands refer directly to the font file names, removing the relative path information:

``` sass Edit Stately's @font-face Command (vendor/assets/stylesheets/_setup.scss)
@font-face {
    font-family: 'stately-webfont';
    // Remove '../font/' from URL load paths, as shown below:
    src: url('stately-webfont.eot');     
    src: url('stately-webfont.eot?#iefix') format('embedded-opentype'),
         url('stately-webfont.woff') format('woff'),
         url('stately-webfont.ttf') format('truetype'),
         url('stately-webfont.svg#testfontRegular') format('svg');
    font-weight: normal;
    font-style: normal;
}
//...
```

And that's it! Now you should be able to use Stately by using the markup as shown in the [example documentation](https://github.com/intridea/stately). For example, to diplay an image of the nation's best state, you would write the following HTML:

``` html How to use Stately In Your Markup
<ul class="stately"> 
  <li data-state="mn" class="MN">X</li>
</ul>
```

What cool stuff have you built with Stately? Leave a link in the comments!

*Charles Worthington (<a href="http://www.twitter.com/cew821" title="Twitter">@cew821</a>) is a freelance product designer living in Washington, DC. He would love to <a href="mailto:%63%6f%6e%74%61%63%74@%67%72%61%79%64%75%63%6b%6c%61%62%73.%63%6f%6d" title="Email Us">hear from you</a>!*