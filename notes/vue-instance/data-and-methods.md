# Data and Methods

Vue instances expose useful instance properties and methods. *These are prefixed with `$` to differentiate them from user-defined properties.*

    var data = { a: 1 }
    var vm = new Vue({
        el: '#example',
        data: data
    })

    vm.$data === data // => true
    vm.$el === document.getElementById('example') // => true

    // $watch is an instance method
    vm.$watch('a', function (newValue, oldValue) {
    // This callback will be called when `vm.a` changes
    })