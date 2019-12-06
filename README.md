# Blibli UI Dev Guideline

Standardization for developing pyeongyang UI with reasonable performance approach


## Getting Started
Before Continue to the content make sure you already read and understand this following concept or tutorials

[Block Element Modifier](http://getbem.com/)

[Airbnb Javascript Style Guide](https://github.com/airbnb/javascript)

[VueJs](https://vuejs.org/)


## Table of Contents

1. CSS 
2. VueJs

## CSS

In Pyeongyang UI we are agreed to use scss
```scss

// bad approach

.header {}
.header__left {}
.header__middle {}
.header__middle__search {}
.header__right {}

// bad approach

.header {}
.left {}
.middle {}
.middle .search {}
.right {}

// good approach

.header {
  &__left {}
  &__middle {
    &__search {}
  }
  &__right {}
}

// best approach
//break the chain if you're already inside element

.header {
   &__left {}
   &__middle {
     .search {}
   }
   &__right {}
 }

```

##Vue Js
### Vue Template
Use VueJs files template by this following order
```vue
<template>
</template>

//use external javascript from js folder
<script src="./js/${fileName}"></script>

//use scss style and scoped the style
<style lang="scss" scoped></style>

```
### Js Template
Use Javascript template by this following order. use as needed. No need create all this lifecycle Hooks!
```js
export default {
  name: 'App',
  components: {},
  data () {
    return {}
  },
  beforeCreate(){},
  created () {
    //Will be execute before the UI being rendered
    //Used for important API Calls
  },
  beforeMount (){},
  mounted () {
    //Will be execute After the UI being rendered
    //Used for DOM manipulation and less important API Calls
  },
  computed: {},
  methods: {},
  watch: {},
  beforeDestroy: {},
  destroyed: {}
}
```

### Import Component
Use Async import for all component (Code Splitting Approach)

[More About Code Splitting](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/code-splitting)
 ```js
const TestComponent = () => import(
  /* webpackChunkName: "c-test-component"*/ '@/components/TestComponent.vue')
export default {
  name: 'Test',
  components: {
    TestComponent
  },
}
```
### Import External Js utils or function
Import only spesific function you want to use (Tree Shaking Approach)

[More About Tree Shaking](https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking)
```js
import { currencyFormat } from '@/utils'
export default {
  name: 'App',
  data() {
    return {
      price: 1000
    }
  },
  computed: {
    priceInIdr() {
      return currencyFormat(this.price)
    }
  }
}
```
### Use Lazy Load image Component from Py-Main
How To install it 
1. npm Install --save supports-webp
2. Add util with name "asset.js"

```js
import supportsWebP from 'supports-webp'

function akamaiImage (url) {
  if (!url) return
  if (!supportsWebP || url.indexOf('.gif') > -1) return url
  const prefix = url.indexOf('?') > -1 ? '&' : '?'
  return url + prefix + 'output-format=webp'
}
export { akamaiImage }
```
3. Add mixins with name "akamai-image-mixin.js"
```js
import { akamaiImage } from '@/util/asset'

export default {
  methods: {
    akamaiImage
  }
}
```
4. Add to mixins to your component Js
```js
import AkamaiImageMixin from '@/mixins/akamai-image-mixin'

mixins: [
    AkamaiImageMixin
  ]
```
5. Use It in your template vue 
```vue
<lazy-image
    class="lazy-image"
    :lazy-src="akamaiImage(test.imageUrl)"
/>
```



