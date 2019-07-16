# Instance Lifecycle Hooks

Each Vue instance goes through series of init steps when created; it needs to set up data observation, compile the template, **mount the instance to the DOM**, and **update** the DOM when data changes. Also runs functions called **lifecycle hooks**, lets users add own code at specific stages.

Example, `created` hook can be used to run code after an instance is created:

    new Vue({
            data: {
            a: 1
        },
        created: function () {
            // `this` points to the vm instance
            console.log('a is: ' + this.a)
        }
    })
    // => "a is: 1"

Other hooks will be called at different stages of instance's lifecycle, e.g., `mounted`, `updated`, and `destroyed`.

Don’t use arrow functions on an options property or callback, such as
    created: () => console.log(this.a) or vm.$watch('a', newValue => this.myMethod())
Since an arrow function doesn’t have a `this`, `this` will be treated as any other variable and lexically looked up through parent scopes until found, often resulting in errors such as
    Uncaught TypeError: Cannot read property of undefined or Uncaught TypeError: this.myMethod is not a function

## Lifecycle Diagram

[![Vue Lifecycle Diagram](/assets/img/vue-lifecycle.png)]