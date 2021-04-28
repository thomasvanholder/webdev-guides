# Guides on How to Code

Bootstrap your productivity as a developer by getting the best out of the coding languages, frameworks and tools you are using every day.

# Contents
1. Languages
   - [HTML](#HTML)
   - [Ruby](#Ruby)
2. Frameworks
   - [Rails](#Rails)
3. Tools
   - [VS Code](#vs-code)

## Languages

### HTML


❌ Don't use infinite div stacking
```html
<div class="mt-5">
  <div class="flex">
    <div>Cities</div>
    <div class="px-5">
      <div>Barcelona</div>
      <div>Paris</div>
      <div>Berlin</div>
    </div>
  <div>
</div>
```
✅ Do use the appropriate HTML elements
```html
<section class="mt-5">
  <div class="flex">
    <h1>Cities</h1>
    <ul class="px-5">
      <li>Barcelona</li>
      <li>Paris</li>
      <li>Berlin</li>
    </ul>
  </div>
</section>
```

---

❌ Don't write 10s of line of code without a linebreak
```html
<section>
  <div></div>
  <div></div>
</section>
<section>
  <div></div>
  <div></div>
</section>
<section>
  <div></div>
  <div></div>
</section>
```
✅ Do use an single enter to visually seperate code
```html
<section>
  <div></div>
  <div></div>
</section>

<section>
  <div></div>
  <div></div>
</section>

<section>
  <div></div>
  <div></div>
</section>
```
---

❌ Don't use `<br>` to break a secentence
```html
<p>This is a sentence which should be split<br>
over multiple lines on mobile</p>
```

✅ Do use ```block``` and ```inline``` to break sentences
```html
<p>This is a sentence which should be split
  <span class="block md:inline">over multiple lines on mobile</span>
</p>
```
---

❌ Don't create custom classes to write CSS
```css
.card-wrapper {
  height: 62px;
  width: 200px;
  margin-top: 8px;
}
```

✅ Do write custom Tailwind inline when needed
```html
<div class="mt-2 h-[62px] w-[200px]">
</div>
```
---

❌ Don't use padding or margin to center and narrow down content
```html
<div class="mx-10 md:mx-20 lg:mx-32 xl:mx-44 2xl: mx-64">
</div>
```

✅ Do use screensize classes to center and narrow down content
```html
<div class="max-w-lg mx-auto">
</div>
```
---
❌ Don't use the margin on every child
```html
<ul>
  <li class="mb-2 mt-2"></li>
  <li class="mb-2 mt-2"></li>
  <li class="mb-2 mt-2"></li>
  <li class="mb-2 mt-2"></li>
  <li class="mb-2 mt-2"></li>
</ul>
```
✅ Do use the space and gap classes
```html
<ul class="space-y-4">
  <li></li>
  <li></li>
  <li></li>
  <li></li>
  <li></li>
</ul>
```
---

❌ Don't use string concatination to create class names
```erb
<div class="text-<%= error ? 'red' : 'green' %>-500"></div>
```

✅ Do dynamically select a complete classname
```erb
<div class="<%= error ? 'text-red-500' : 'text-green-500' %>"></div>
```

Tailwind uses PurgeCSS which drops all un-used classes in production to keep the CSS file size small. Concatinated are not picked up.

---

❌ Don't use center
```html
```

✅ Do use inset to position an image
```html
```

---

❌ Don't use flex for positioning elements rows and columns
```html
<ul class="flex flex-col items-center">
  <li class="flex">
    <div>City</div>
    <div>Barcelona</div>
  </li>
  <li class="flex">
    <div>Country</div>
    <div>Spain</div>
  </li>
</ul>
```

✅ Do use grid to display columns and rows
```html
<ul class="grid grid-cols-2">
  <li class="flex">
    <div>City</div>
    <div>Barcelona</div>
  </li>
  <li class="flex">
    <div>Country</div>
    <div>Spain</div>
  </li>
</ul>
```

Flexbox is great for horizontal or vertical content.

---

❌ Don't add in svg images inside the html code
```html
<svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z" />
</svg>
```

✅ Do store the images in the approriate assets folder
```
app > assets > images
```
In production your code can break when not mentioning the extension.

---

❌ Don't use the image_tag without the image extension
```erb
<%= image_tag "svg/credit_card" %>
```

✅ Do use the image tag of any images you use
```erb
<%= image_tag "svg/credit_card.svg" %>
```

---

---
❌ Don't use the ```<b>``` element to style text
```html
<p>these are <b>highlighted words</b> in a sentence.</p>
```

✅ Do use a font-famliy to add styles to text
```html
<p>these are <span class="font-bold">highlighted words</span> in a sentence.</p>
```

---


## TODO
[] div element is an element of last resort.
[] article for contact information
[] Avoid s, i, b, and u elements





### Ruby

For a full guide read the [Ruby Style Guide](https://rubystyle.guide/)

## Frameworks

### Rails

For a full guide read the [Rails Style Guide](https://rails.rubystyle.guide/)

## Tools

### VS Code
[Keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
