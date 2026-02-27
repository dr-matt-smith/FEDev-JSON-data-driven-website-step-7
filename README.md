Back to FEDev main <<<< https://github.com/dr-matt-smith/2026-FDEev-Front-End-Dev-start-here

JSON-driven website: 
[Step 1](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-1)
|
[Step 2](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-2)
|
[Step 3](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-3)
|
[Step 4](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-4)
|
[Step 5](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-5)
|
[Step 6](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-6)
|
[Step 7](https://github.com/dr-matt-smith/FEDev-JSON-data-driven-website-step-7)


# FEDev-JSON-data-driven-website-step-4

![module list from JSON](/screenshots/6_all_modules_from_JSON.png)

Step-by-step development of a module details website. Routing in the form `/module/2037`


## 1: JavaScript server 404-page redirect

![404 Not found page for bad module ID](/screenshots/5_not_found_404_page.png)

We can further simplify our module details page, so assume that it will only be used to render a page if a module was successfully found.

We can make this assumption, since in our module details JavaScript server page, we can test to see if a module was NOT found, and if so, geneate a 404 Not Found error page.

Let's add this check and 404 redirect to our module details JavaScript server page (`/modules/[moduleCode]/+page.server.js`):

```javascript
import { error } from '@sveltejs/kit';
import modules from '$lib/data/modules.json';

export function load({ params }) {
   let moduleCode = parseInt(params.modulecode);

    const module = modules.find(module =>
        module.id === moduleCode
    );

    // if module object underfined, we didn't find one,
    // so generate a 404 NOT FOUND redirect
    if (!module) error(404);

    return {
        module
    };
}
```

This means the code for our module details Svelte page (`/modules/[moduleCode]/+page.svelte`) is much simpler, since we don't have to check whether a module object is undefined or not. We just get the module object

```html
<script>
    let { data } = $props();
    let module = data.module;
</script>

<h1>
    Details of module with code = {module.id}
</h1>

<h3>{module.title}</h3>
<p>
    {module.details}
</p>
```
