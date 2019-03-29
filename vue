var Vue = function (vm) {
  initProxy(vm)
  initGlobalAPI(vm)
  initEvent(vm)
  initEl(vm)
}
var initProxy = function (vm) {
  for (const sourceKey in vm) {
    if (sourceKey === 'el') {
      continue
    }
    var data = vm[sourceKey]
    var keys = Object.keys(data)
    for (let i = 0, key; key = keys[i++];) {
      Object.defineProperty(vm, key, {
        enumerable: true,
        configurable: true,
        get: function () {
          return this[sourceKey][key]
        },
        set: function (val) {
          this[sourceKey][key] = val
        }
      })
    }
  }
}
var initGlobalAPI = function (vm) {
  vm.$el = document.querySelector(vm.el) || ''
  vm.$data = vm.data
  vm.$nextTick = function (fn) {
    setTimeout(fn)
  }
}
var initEvent = function (vm) {
  var hooks = ['beforeCreate', 'created', 'beforeMount', 'mounted', 'beforeUpdate', 'updated', 'beforeDestroy', 'destroyed']
  for (var i = 0, hook; hook = hooks[i++];) {
    vm[hook] && vm[hook]()
  }
}
var initEl = function (vm) {
  if (!vm.el) {
    return
  }
  vm.$el.innerHTML = vm.$el.innerText.replace(/{{(.+?)}}/g, function (params, p1) {
    return vm[p1]
  })
}
new Vue({
  el: '#app',
  data: {
    msg: 'Hello World'
  },
  methods: {
    hello: function () {
      console.log(this.msg)
    }
  },
  created: function () {
    this.hello()
    this.$nextTick(() => {
      console.log(this.$el)
    })
    console.log(this)
  }
})
