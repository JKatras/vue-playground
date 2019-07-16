# Interpolations

## Text

Most basic form of data binding is "moustache" syntax:

    <span>Message: {{ msg }}</span>

{{ }} will be replaced with value of `msg` property on corresponding data object.  It will also be updated when data object's `msg` property changes.

One-time interpolations that do not update on data change use `v-once directive`; keep in mind this will affect any other bindings on the same node.

## Raw HTML

{{ }} inteprets data as plain text, not HTML. To output real HTML, use `v-html` directive:

    <p>Using moustaches: {{ rawHtml }}</p>
    <!-- Using moustaches: <span style="color:red"> -->

    <p>Using v-html directive: <span v-html="rawHtml"></span></p>
    <!-- rawHtml is interpolated to HTML -->

**Only use HTML interpolation on trusted content - never on user-provided content**

## Attributes

{{ }} can't be used inside HTML attributes. Instead, use `v-bind directive`

    <div v-bind:id="dynamicId"></div>

In the case of boolean attributes, where their mere existence implies true, v-bind works a little differently. In this example:

    <button v-bind:disabled="isButtonDisabled">Button</button>

If `isButtonDisabled` has the value of null, undefined, or false, the disabled attribute will not even be included in the rendered <button> element.

## Directives

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be **a single JavaScript expression** (with the exception of `v-for`, which will be discussed later). A directive's job is to reactively apply side effects to the DOM when the value of its expression changes. Let's review the example we saw in the introduction:

``` html
<p v-if="seen">Now you see me</p>
```

Here, the `v-if` directive would remove/insert the `<p>` element based on the truthiness of the value of the expression `seen`.

### Arguments

Some directives can take an "argument", denoted by a colon after the directive name. For example, the `v-bind` directive is used to reactively update an HTML attribute:

``` html
<a v-bind:href="url"> ... </a>
```

Here `href` is the argument, which tells the `v-bind` directive to bind the element's `href` attribute to the value of the expression `url`.

Another example is the `v-on` directive, which listens to DOM events:

``` html
<a v-on:click="doSomething"> ... </a>
```

Here the argument is the event name to listen to. We will talk about event handling in more detail too.

### Dynamic Arguments

> New in 2.6.0+

Starting in version 2.6.0, it is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets:

``` html
<a v-bind:[attributeName]="url"> ... </a>
```

Here `attributeName` will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument. For example, if your Vue instance has a data property, `attributeName`, whose value is `"href"`, then this binding will be equivalent to `v-bind:href`.

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

``` html
<a v-on:[eventName]="doSomething"> ... </a>
```

Similarly, when `eventName`'s value is `"focus"`, for example, `v-on:[eventName]` will be equivalent to `v-on:focus`.

#### Dynamic Argument Value Constraints

Dynamic arguments are expected to evaluate to a string, with the exception of `null`. The special value `null` can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

#### Dynamic Argument Expression Constraints

<p class="tip">Dynamic argument expressions have some syntax constraints because certain characters are invalid inside HTML attribute names, such as spaces and quotes. You also need to avoid uppercase keys when using in-DOM templates.</p>

For example, the following is invalid:

``` html
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```

The workaround is to either use expressions without spaces or quotes, or replace the complex expression with a computed property.

In addition, if you are using in-DOM templates (templates directly written in an HTML file), you have to be aware that browsers will coerce attribute names into lowercase:

``` html
<!-- This will be converted to v-bind:[someattr] in in-DOM templates. -->
<a v-bind:[someAttr]="value"> ... </a>
```

### Modifiers

Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the `.prevent` modifier tells the `v-on` directive to call `event.preventDefault()` on the triggered event:

``` html
<form v-on:submit.prevent="onSubmit"> ... </form>
```

You'll see other examples of modifiers later, [for `v-on`](events.html#Event-Modifiers) and [for `v-model`](forms.html#Modifiers), when we explore those features.

## Shorthands

The `v-` prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the `v-` prefix becomes less important when you are building a [SPA](https://en.wikipedia.org/wiki/Single-page_application), where Vue manages every template. Therefore, Vue provides special shorthands for two of the most often used directives, `v-bind` and `v-on`:

### `v-bind` Shorthand

``` html
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```

### `v-on` Shorthand

``` html
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

They may look a bit different from normal HTML, but `:` and `@` are valid characters for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.