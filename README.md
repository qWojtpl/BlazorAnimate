# BlazorAnimate

### Simple library to bring life to your MAUI Blazor project

<br>

# Getting started

1. Download NuGet package:

```
Install-Package qWojtpl.BlazorAnimate
```

2. Go to wwwroot/index.html and add this line to header:

```html
<link rel="stylesheet" href="_content/qWojtpl.BlazorAnimate/css/bundle.css" />
```

> This should be the first stylesheet you are loading in this document!

> Bundle contains two files: pages.css and animations.css

> animations.css adds all animations

> pages.css adds styles for pages

<br>

3. Add imports to _Imports.razor

```cshtml
@using BlazorAnimate;
@using static BlazorAnimate.PageLoader.AnimationType;
```

4. Create main component

- Add empty component to your project (eg. Main.razor)
- Main component should inherit from AppLoader
- This should be defualt page in your application

```cshtml
@page "/"
@inherits PageLoader
```

5. Prepare first page

- Create new component (eg. Login.razor)
- This page should inherit from AppPage
- You need to add object of page in this component
- In style attribute of this page you have to add reference to PageStyles with index as name of this component

```cshtml
@page "/login"
@inherits AppPage

<div id="your-page-id" class="page" style="@Loader.PageStyles["Login"]">
    
    /* Page html */

</div>

```

> You have to use class page - this is class from pages.css

> You have to use style="@Loader.PageStyles["ComponentName"]"~

> Don't forget to set background color to your page using id!

<br>

6. Add component to Main component

- In your main component (which inherits from PageLoader) add reference to component

```cshtml
<Login AnimationIn="AnimationType.FADE_IN" AutoLoad="true" Loader="this" />
```

> Loader should be always at the end of component reference!

7. Run application and see the results

<br>

# Inner pages

- In default, BlazorAnimate only lets you create control to one-deep pages (load next <-> unload latest)
- If you want to have page control inside page you can use inner pages

```cshtml
Loader.Load(<PageName>, true);
                        ^^^^
                Marks page as inner page
```

- For more methods, see Methods section

<br>

# Methods

<details><summary><h3 style="display:inline-block;">Loading next page</h2></summary>

```cshtml
<button @onclick="(() => Loader.Load(<PageName>))">Next page!</button>
```

</details>

<details><summary><h3 style="display:inline-block;">Unload page</h2></summary>

- UnloadLatest doesn't load previous page, only unloads current page!
- Previous page must stay in this same position, so second page should have PreviousAnimationOut as none or first page shouldn't have AnimationOut 

```cshtml
<button @onclick="Loader.UnloadLatest">Close me!</button>
```

</details>

<details><summary><h3 style="display:inline-block;">Unloading all pages</h2></summary>

- Unloads all pages and loads AutoLoad page

```cshtml
<button @onclick="Loader.UnloadAll">Unload all pages!</button>
```

</details>

<details><summary><h3 style="display:inline-block;">Load inner page</h2></summary>

```cshtml
<button @onclick="(() => Loader.Load(<PageName>, true))">Load page as inner page</button>
```

</details>

<details><summary><h3 style="display:inline-block;">Unload latest inner page</h2></summary>

- UnloadLatestInner doesn't load previous inner page, only unloads current inner page!
- Previous inner page must stay in this same position, so second page should have PreviousAnimationOut as none or first page shouldn't have AnimationOut 

```cshtml
<button @onclick="Loader.UnloadLatestInner">Unload latest inner page</button>
```

</details>

<details><summary><h3 style="display:inline-block;">Unload all inner pages</h2></summary>

```cshtml
<button @onclick="Loader.ExitInner">Exit all inner pages!</button>
```

</details>

<br>

# Component and animations

<details><summary><h3 style="display:inline-block;">Component attributes</h2></summary>


## AnimationIn

- Choose enum from AnimationType
- This animation will be fired when component is loading

## AnimationOut

- Choose enum from AnimationType
- When this component will be unloaded 

## PreviousAnimationOut

- If you want previous animation to disappear in different way you can set PreviousAnimationOut

## AutoLoad

- True or false, this component will load automatically.
- Only one component should have AutoLoad!

## Loader

- Set this in main component as this
- Calling this attribute will register this component

<br>


</details>

<details><summary><h3 style="display:inline-block;">Animation list</h2></summary>

```css
- SLIDE_FROM_RIGHT 
- SLIDE_TO_RIGHT 
- SLIDE_FROM_LEFT 
- SLIDE_TO_LEFT 
- SLIDE_FROM_BOTTOM 
- SLIDE_TO_BOTTOM 
- SLIDE_FROM_TOP
- SLIDE_TO_TOP 
- FADE_IN 
- FADE_OUT 
```

</details>

<br>

# Known performance issues

- When unloading page its display is not changing to none - if you load houndreds of pages, its visibility will never be changed to none, so it will be still rendering in the background
