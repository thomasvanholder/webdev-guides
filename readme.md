# Guides on How to Code

Bootstrap your productivity as a developer by getting the best out of the coding languages, frameworks and tools you are using every day.

## Languages

### HTML with Tailwind

❌ Don't use infinite divs
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
<div class="mt-2 h-[62px] w-[200[px]">
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

❌ Don't write custom classes to write CSS
```css
.card-wrapper {
  height: 62px;
  width: 200px;
  margin-top: 8px;
}
```

✅ Do write custom Tailwind inline when needed
```html
<div class="mt-2 h-[62px] w-[200[px]">
</div>
```

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





### Ruby

For a full guide read the [Ruby Style Guide](https://rubystyle.guide/)

## Frameworks
For a full guide read the [Rails Style Guide](https://rails.rubystyle.guide/)

## Tools

### VS Code
[Keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
