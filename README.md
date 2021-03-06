#### Current stable version: 0.1.7

# NgResponsiveNav

This is a fully responsive navigation menu solution. It comes with a desktop navigation menu and collapses into a mobile burger menu.
This library is for use with Angular 8 only, as I haven't tested it in older versions of Angular (or 9). It has minimal dependencies, no Bootstrap, just Angular, HTML, SCSS and the amazing
[Font Awesome Angular Module](https://www.npmjs.com/package/@fortawesome/angular-fontawesome)

#### Recent Improvements
* Fixed issue where this just didn't work as an NPM package, a built version of the 
Angular library was required to use the package. This was due to the way exports were being 
managed in public-api.ts

* Improved css for nav-links: At the time of upload, there were no css rules being applied to url links
* Allowed for further customisation of Desktop nav through the use of ng-content
* Allowed for further customisation of Mobile nav through the use of ng-content.
* Improved the configuration object by allowing a colour palette. 
* Allowed the choice for box shadow on the desktop navigation

## Installation

You will need to have the following packages in your Angular app's package.json file: 

``` 
"@fortawesome/angular-fontawesome": "^0.5.0",
"@fortawesome/fontawesome-svg-core": "^1.2.26",
"@fortawesome/free-solid-svg-icons": "^5.12.0",
```

Once you have pasted these into dependencies, like so

```JSON
{
  "name": "ng8-responsive-nav-bar-test-bed",
  "version": "0.0.0",
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e"
  },
  "private": true,
  "dependencies": {
    "@angular/animations": "~8.2.9",
    "@angular/common": "~8.2.9",
    "@angular/compiler": "~8.2.9",
    "@angular/core": "~8.2.9",
    ...
    "@fortawesome/angular-fontawesome": "^0.5.0",
    "@fortawesome/fontawesome-svg-core": "^1.2.26",
    "@fortawesome/free-solid-svg-icons": "^5.12.0",
  },
  ...,
}
```
run ```npm i``` to install the depenedencies.

Use NPM to install this navigation menu package

```npm i ng-responsive-nav```

## How To Use
The navigation menu comes with a configuration object that is then used to style and populate the menu with route paths. 

```TypeScript
export abstract class NavConfig {
      public readonly navColourConfig?: NavBrandConfig;
      public readonly navigationMenuRoutes?: Array<NavigationMenuRoute>;
      public readonly navigationHomeRoute: NavigationMenuRoute;
}
```

The object is simple enough. For now the navigation menu can only accept parent routes as it's still a work in progress. 
To use the navigation Module, you must declare it in app.module (or your shared module) like so: 

```TypeScript

import { NavConfig, NgResponsiveNavModule } from 'ng-responsive-nav';

export const navConfig: NavConfig = {
  navBrandConfig: {
    colorPalette: {
      primaryColor: '#1976D2',
      secondaryColor: '#ffffff',
      linkActiveColor: '#1976D2',
      linkColor: '#1976D2',
      tertiaryColor: '#1976D2',
      backGroundColor: '#ffffff'
    },
    brandImageUrl: 'https://placehold.it/300x300',
    zIndex: 1,
    boxShadow: true,
  },
  navigationHomeRoute: {
    routePath: '/',
    routeName: 'home'
  },
  navigationMenuRoutes: []
};

@NgModule({
  imports: [NgResponsiveNavModule.forRoot(navConfig)],
  exports: [NgResponsiveNavModule]
})

export class SharedModule {}

```

This library will work without your configuration as it uses a factory to create styles and branding from either you (the user) or 
a default config object. This object is identical to the one declared in the above markdown. 

The ```forRoot()``` method however, does definitely require a ```NavConfig``` with at least a home route to work. So: 

```javascript
import { NavConfig, NgResponsiveNavModule } from 'ng-responsive-nav';

const navConfig: NavConfig = {
  navigationHomeRoute: {
    routePath: '/',
    routeName: 'home'
  },
  navigationMenuRoutes: []
};

@NgModule({
  imports: [NgResponsiveNavModule.forRoot(navConfig)],
  exports: [NgResponsiveNavModule]
})
```

is also valid

Then on your component, simply declare it like any other ngModule: 

```html
<ng-responsive-nav></ng-responsive-nav>
<router-outlet></router-outlet>
```

I use it on top of my apps main router outlet, but it can be declared anywhere. 

## Customising Desktop Nav
This is a feature that allows you to add extra content on the desktop nav bar. This can be something like a CTA button for 
login, etc. 

To Add the extra content, you can do so like this: 

```html
<!-- Declare the ng-responsive-nav -->

<ng-responsive-nav>
    <!-- ngDesktopExtras is the selector used  -->
    <!-- the extra content -->
  <div ngDesktopNavExtras>
    <button>Test button</button>
  </div>

</ng-responsive-nav>
```

And it will appear on the right hand side of the menu like the image below: 

## Customising Mobile Nav
This works the same way as Desktop only this time, the content selector is 
```ngMobileNavExtras```

To Add the extra content, you can do so like this: 

```html
<!-- Declare the ng-responsive-nav -->
<ng-responsive-nav>
  <div ngMobileNavExtras>
    <div style="color: black">
      <h1>Hello</h1>
    </div>
  </div>
</ng-responsive-nav>
```
The content will appear at the bottom of the mobile navigation menu

## Customising both

You can customise both of the navigation menus at the same time, it will look something like this 

```html
<ng-responsive-nav>
  <div ngDesktopNavExtras>
    <button>Test button</button>
  </div>

  <div ngMobileNavExtras>
    <div style="color: black">
      <h1>Hello</h1>
    </div>
  </div>
</ng-responsive-nav>
```

and it will apply to the correct menu.

## WINDOW Provider

Another useful feature this library provides is an Injection token for the browser window. It provides a safe way of 
interacting with the browser window read more [here](https://brianflove.com/2018/01/11/angular-window-provider/). 

You can use the token like so: 

```TypeScript

import { WINDOW } from 'ng-responsive-nav';

@Component(...)
export class DemoClass implements OnInit {
    
    private _window: Window;
    
    constructor(@Inject(WINDOW) window: Window) {
        this._window = window;
    }

    public ngOnInit(): void {
        /** Add an event listener*/
        this._window.addEventListneer('orientationchange', () => ...)
    }
}
```

### Demo: 

You can see a live version of this working over at [my blog](https://marcfreemandev.web.app/home)

### Future Plans

There's still a lot of things I'd like to do with this library

1. Using the RouterModule rather than custom routes in forRoot. 
2. Custom SVGs to completely remove dependencies from the library.
3. Deeper customisation, using content transclusion.
4. Unit and E2E tests, I want 100% coverage.

### Bugs 
You can report any bugs over at the [GitHub Repository](https://github.com/Dudecor3/ng-responsive-nav) and I'll get on them right away.
