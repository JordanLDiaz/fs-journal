# Advanced Front-End Frameworks


**1.** Describe the two ways to bind Data in Vue?
<!-- enter you answer in the space below -->
```
You can bind data using v-bind (:) which binds class attributes to elements.
You can also use interpolation {{}} to render data dynamically to the page.
```

**2.** The `SPA` acronym stands for what?
<!-- enter you answer in the space below -->
```
Single page application.
```
**3.** What are some of the advantages/uses of a `SPA` over a traditional one?
<!-- enter you answer in the space below -->
```
Using a SPA allows webpages to load more quickly. They are also more responsive than MPA's.
```
**4.** What does the `onMounted` method in Vue do?
<!-- enter you answer in the space below -->
```
OnMounted calls methods on page load. 
```
**5.** What is the `v-model` attribute in Vue for, and when might you use it?
<!-- enter you answer in the space below -->
```
V-model is used to 2-way data bind values in our templates with values in our data properties. We use v-models in forms and input elements.
```
**6.** The `v:on` (`@`) directive can be used for what?
<!-- enter you answer in the space below -->
```
'v-on' is used like an onclick/onsubmit method from MVC. It gives directions to a method that will occur when the user does something, like click a button. 
```
**7.** Which Vue attributes(directives) could you use to conditionally render elements on a page?
<!-- enter you answer in the space below -->
```
To conditionally render elements to the page, we use v-if, v-else, v-else-if, and v-show. 
```
**8.** What is the purpose of the `key` attribute when using `v-for` on an element?
<!-- enter you answer in the space below -->
```
The key attribute with a v-for lets you give elements on the page a unique attribute, often an id, that lets data be manipulated and rerendered without concern that data will be lost. 
```
**9.** What is the `<slot>` element and what is it used for?
<!-- enter you answer in the space below -->
```
You use <slot> tags when injecting modals into a template, which allows you to reuse the same component, but inject other components within it via a slot tag. 
```