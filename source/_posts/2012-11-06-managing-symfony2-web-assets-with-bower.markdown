---
layout: post
title: "Managing web assets in Symfony2 with Bower"
date: 2012-11-05 22:43
comments: true
categories:
---
In a typical Symfony2 application, web assets are handled elegantly by taking advantage of the `asset` Twig function or by using another library, called [Assetic](https://github.com/kriswallsmith/assetic), which allows you to [do much more interesting things](http://symfony.com/doc/current/cookbook/assetic/asset_management.html) with those assets.

If you follow the bundles directory structure conventions, then you should put your web assets (images, scripts, stylesheets, etc) in the `Resources/public/` directory inside your bundle.

Although Assetic and the `asset` function will do the rest of the work for you (copying, linking, combining, filtering and dumping assets) you still have to manage your assets dependencies by hand.

[Composer](http://getcomposer.org/) become the core dependency manager for Symfony 2.1 and beyond. It is a dependency management library for PHP, it was created for handling PHP dependencies, but NOT for handling front-end (assets) dependencies.

So, how do we manage the dependencies of our web assets?

Having to manually download and keep updated all front-end dependencies is a very tedious task.

## Bower to the rescue

[Bower](http://twitter.github.com/bower/) is a package manager for the web. Bower lets you easily install assets such as images, CSS and JavaScript, and manages dependencies for you.

Bower is **just** a package manager, and nothing else. It does not offer the ability to combine, concatenate or minify code, it’s sole purpose is to manage packages.

## Using the Bower component.json file

Just as we have a `composer.json` file with our PHP dependencies, you can create a `component.json` file in your project's root, specifying all front-end dependencies.

``` javascript Sample component.json file.
{
	"name": "Acme Application",
    "version": "1.0.0",

    "dependencies": {
    	"jquery": "~1.7.2",
    	"bootstrap": "latest",
 		"undersocre": "~1.4.2"
  	}
}
```

[Install Bower](https://github.com/twitter/bower#installing-bower) in your development environment, and the same way we do with Composer, do `bower install` or `bower update`.

By default, Bower will install packages in the `components` directory, so we need to change its default configuration to follow Symfony2 conventions.

## Configuring Bower for Symfony2 applications

Bower can be configured by creating a `.bowerrc` file in your project´s root with one or all of the following configuration parameters:

``` json .bowerrc configuration file
{
    "directory" : "src/Acme/DemoBundle/Resources/public",
    "json"      : "component.json",
    "endpoint"  : "https://bower.herokuapp.com"
}
```

With the above configuration, once you do `bower install` or `bower update`, the packages will be copied to the `src/Acme/DemoBundle/Resources/public` directory as specified in the `.bowerrc` file.

## Using the packages installed by Bower

Remember, Bower is just a package manager, so there’s no Bower-specific way to use these packages. So, we’ll just stick with the Symfony2 way of linking to assets in the bundle `Resources/public` directory (using `asset` or Assetic).

We just reference the assets as we have always do.

```html+jinja
    {% stylesheets
        '@AcmeDemoBundle/Resources/public/bootstrap/less/bootstrap.less'
        '@AcmeDemoBundle/Resources/public/bootstrap/less/responsive.less'
    %}
    <link rel="stylesheet" href="{{ asset_url }}" />
    {% endstylesheets %}
```

## Conclusion and next steps

So, that´s all. Bower just will help us to manage our client-side/front-end dependencies in a Symfony2 application or inside our bundle.

The next challenge would be to include Bower commands in our deploy process so that we can have packages automatically downloaded and installed during deploys.

Dependencies management sucks, that´s a fact. We already have composer to manage PHP libraries and bundles dependencies. I am very happy to have an elegant client-side dependency manager as well!

## References
- [Bower: The browser package manager](http://twitter.github.com/bower/)
- [Meet Bower: A Package Manager For The Web](http://net.tutsplus.com/tutorials/tools-and-tips/meet-bower-a-package-manager-for-the-web/)



