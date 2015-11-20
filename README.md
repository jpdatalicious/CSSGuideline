
### Opinionated Datalicions Front-end Best Practice Guidelines ###

**Goal**
* Foster code consistency
* Ease of maintenance
* Ensure create professional grade front-end
* Guide staff on-boarding or educate new developers



### Think in components ###
---
Consider each piece of your UI as an individual "component".

### Naming components ###
---

Components will be named with at least two words, prefixed with `st`, separated by a dash. Examples of components:

 * A like button (.st-like-button)
 * A search form (.st-search-form)
 * A news article card (.st-article-card)


### One component per file ###
___

```less
/* css/components/search-form.less */
.st-search-form {
  > .st-button { /* ... */ }
  > .st-field { /* ... */ }
  > .st-label { /* ... */ }

  // variants
  &.-small { /* ... */ }
  &.-wide { /* ... */ }
}
```

### Elements ###
___
Elements are things inside your component.

### Naming Elements ###
___
Each component may have elements. They should have classes that are only *one word.* prefixed with `st` to avoid namespace coallition.

```less
.st-search-form {
  > .st-field { /* ... */ }
  > .st-btn-cancel { /* ... */ }
}
```

### Avoid tag selectors ###
---
Use classnames whenever possible. Tag selectors are fine, but they may come at a small performance penalty and may not be as descriptive.

```less
.st-article-card {
  > h3    { /* ✗ avoid */ }
  > .name { /* ✓ better */ }
}
```


### Avoid over nesting ###
___
Use no more than 1 level of nesting. It's easy to get lost with too much nesting.


```less
/* ✗ Avoid: 3 levels of nesting */
.st-image-frame {
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

### Avoid positioning properties ###
---
Components should be made in a way that they're reusable in different contexts. Avoid putting these properties in components:
 * Positioning (position, top, left, right, bottom)
 * Floats (float, clear)
 * Margins (margin)
 * Dimensions (width, height) 

### Define positioning in parents ###
___
If you need to define these, try to define them in whatever context they will be in. In this example below, notice that the widths and floats are applied on the list component, not the component itself.


```less
.article-list {
  & {
    @include clearfix;
  }

  > .article-card {
    width: 33.3%;
    float: left;
  }
}

.article-card {
  & { /* ... */ }
  > .image { /* ... */ }
  > .title { /* ... */ }
  > .category { /* ... */ }
}
```

### Fixed dimensions ###
___
Exception to these would be elements that have fixed width/heights, such as avatars and logos.


### Bleeding through nested components ###
___
Be careful about nested components with elements sharing the same name as elements in its container.

```html
<article class='st-article-link'>
 <div class='st-vote-box'>
    <button class='st-up'></button>
    <button class='st-down'></button>
    <span class='st-count'>4</span>
  </div>

  <h3 class='st-title'>Article title</h3>
  <p class='st-count'>3 votes</p>
</article>
```

```less
.st-article-link {
  > .st-title { /* ... */ }
  > .st-count { /* ... (!!!) */ }
}

.st-vote-box {
  > .st-up { /* ... */ }
  > .st-down { /* ... */ }
  > .st-count { /* ... */ }
}
```

In this case, if `.st-article-link > .count` did not have the `>` (child) selector, it will also apply to the `.st-vote-box .st-count` element. **This is one of the reasons why child selectors are preferred**.


### How do you apply margins outside a layout? Try it with Helpers. ###
___
For general-purpose classes meant to override values, put them in a separate file and name them beginning with an underscore. They are typically things that are tagged with !important. Use them very sparingly.

```less
._unmargin { margin: 0 !important; }
._center { text-align: center !important; }
._pull-left { float: left !important; }
._pull-right { float: right !important; }
```

### Naming helpers
---
Prefix classnames with an underscore. This will make it easy to differentiate them from modifiers defined in the component. Underscores also look a bit ugly which is an intentional side effect: using too many helpers should be discouraged.

```less
<div class='order-graphs -slim _unmargin'>
</div>
```

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

### Namespace your classes ###
___
Namespacing your classes keeps your components self-contained and modular. It minimizes the likelihood that an existing class will conflict, and it lowers the specificity required to style child elements.

```less
/* High risk of style cross-contamination */
.st-widget { }
.st-widget .title { }

/* Low risk of style cross-contamination */
.st-widget { }
.st-widget-title { }
```

Supertag's namespace is `st-`.

### Simplifying nested components ###
---
Sometimes, when nesting components, your markup can get dirty:

```html
<div class="st-modal-footer">
  <button class="btn btn-raised cancel" data-dismiss="modal">Cancel</button>
</div>
```

You can simplify this by using your CSS preprocessor's @extend mechanism:

```html
<div class="modal-footer">
  <button class="cancel" data-dismiss="modal">Cancel</button>
</div>
```

```less
.st-modal-footer {
      background: #333;

      > .cancel {
          &:extend(.btn);
          color: #333;
          background: #fff;

          &:hover {
            &:extend(.btn:hover);
          }
      }
}      
```

*Caveat*
You need to import the less file in case you're extending a class not declared in the same file 
e.g. `@import "../../bootstrap/buttons.less";` for importing `btn` Bootstrap class.



### Create your HTML First ###
___
Many designers create their CSS at the same time they create the HTML. It seems logical to create both at the same time, but actually you’ll save even more time if you create the entire HTML mockup first. The reasoning behind this method is that you know all the elements of your site layout, but you don’t know what CSS you’ll need with your design. Creating the HTML layout first allows you to visualize the entire page as a whole, and allows you to think of your CSS in a more holistic, top-down manner


### Apprehensions ###
___
Some people may have apprehensions to these conventions, such as:


####"But dashes suck"####

You're free to omit them and just use regular words, but keep the rest of the ideas in place (components, elements, variants).

####"But I can't think of 2 words!"####

Some components will only need one word to express their purpose, such as alert. In these cases, consider that using some suffixes will make it clearer that it's a block-level element:

* .alert-box
* .alert-card
* .alert-block
* 
Or for inlines:

* .link-button
* .link-span


### Summary ###
---

* Think in components, named with 2 words (.screenshot-image)
* Components have elements, named with 1 word (.blog-post > .title)
* Name variants with a dash prefix (.shop-banner.-with-icon)
* Components can nest
* Remember you can extend to make things simple


Credits: http://ricostacruz.com/rscss/index.html
