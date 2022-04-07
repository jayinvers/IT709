# 07 Foreground app with .NET 6.0 MVC architecture (part 5) Focus on the templates

## 0. HTML shortcut with **VS Code**

When we edit the HTML file, I would like to use the VS Code. There are so many extensions that could accelerate the productivity of coding with HTML or CSS. Here are couples of the following usefull extensions.

### Useful Extension

#### HTML & CSS

- GitLens
- prettier
- Color manager
- **CSS Peek**
- Image preview
- path Intellisense
- **html to css autocompletion**
- **IntelliSense for CSS class names in HTML**
- Live Share
- bracket padder
- bracket colorizer
- Import Cost
- **Indent rainbow**
- **Auto Rename tags**
- SVG Viewer

#### Front-end related extensions

- REST Client
- Tabnine
- TODO Highlight
- vscode-styled-components
- Paste JSON as Code
- project manager
- code time
- emmet
- vscode faker
- ESlint Chinese Rule
- power mode

#### CSV

- rainbow csv
- JSON to CSV
- quotify

### Snippets in Vs Code

Creating a checkbox in vscode is not intuitive, so we need to write a lot of code for it.

<https://snippet-generator.app/>

<https://code.visualstudio.com/docs/editor/userdefinedsnippets#_variables>

## 1. Web Font & Icon

Usually some special fonts need corresponding font file support, which can be achieved by referencing google fonts.

Google Fonts
<https://fonts.google.com/>

- Add font to html file

```html
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Catamaran:100,200,300,400,500,600,700,800,900">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato:100,100i,300,300i,400,400i,700,700i,900,900i">
```

- And add to css

```css
@import url(https://fonts.googleapis.com/css?family=Catamaran:100,200,300,400,500,600,700,800,900);
@import url(https://fonts.googleapis.com/css?family=Lato:100,100i,300,300i,400,400i,700,700i,900,900i);

/* Example */
body{
    font-family: "Lato";
}
```

## 2. Layout

- nav
- .hero-container
- header
- footer
- section

## 3. Nav

In the presentation.
## 4. Hero-Banner

https://getbootstrap.com/docs/5.1/examples/heroes/

## 5. Section and container

https://getbootstrap.com/docs/5.1/examples/
