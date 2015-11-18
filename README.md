

### One component per file ###
```less
/* css/components/search-form.less */
.search-form {
  > .button { /* ... */ }
  > .field { /* ... */ }
  > .label { /* ... */ }

  // variants
  &.-small { /* ... */ }
  &.-wide { /* ... */ }
}
```

### Avoid over nesting ###
___
Use no more than 1 level of nesting. It's easy to get lost with too much nesting.


```less/
/* ✗ Avoid: 3 levels of nesting */
.image-frame {
  > .description {
    /* ... */

    > .icon {
      /* ... */
    }
  }
}

/* ✓ Better: 2 levels */
.image-frame {
  > .description { /* ... */ }
  > .description > .icon { /* ... */ }
}
```

### Bleeding through nested components ###
___
Be careful about nested components with elements sharing the same name as elements in its container.

```html
<article class='article-link'>
 <div class='vote-box'>
    <button class='up'></button>
    <button class='down'></button>
    <span class='count'>4</span>
  </div>

  <h3 class='title'>Article title</h3>
  <p class='count'>3 votes</p>
</article>

```

```less
.article-link {
  > .title { /* ... */ }
  > .count { /* ... (!!!) */ }
}

.vote-box {
  > .up { /* ... */ }
  > .down { /* ... */ }
  > .count { /* ... */ }
}
```

In this case, if .article-link > .count did not have the > (child) selector, it will also apply to the .vote-box .count element. This is one of the reasons why child selectors are preferred.


### Use Shorthand CSS ###

```less
margin: 10px 24px 12px 0;
```
is favored over

```less
margin-top: 10px;
margin-right: 24px;
margin-bottom: 12px;
margin-left: 0;
```


### Create your HTML First ###
___
Many designers create their CSS at the same time they create the HTML. It seems logical to create both at the same time, but actually you’ll save even more time if you create the entire HTML mockup first. The reasoning behind this method is that you know all the elements of your site layout, but you don’t know what CSS you’ll need with your design. Creating the HTML layout first allows you to visualize the entire page as a whole, and allows you to think of your CSS in a more holistic, top-down manner


### Summary ###
---

* Think in components, named with 2 words (.screenshot-image)
* Components have elements, named with 1 word (.blog-post > .title)
* Name variants with a dash prefix (.shop-banner.-with-icon)
* Components can nest
* Remember you can extend to make things simple
