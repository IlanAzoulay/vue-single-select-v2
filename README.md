# vue-single-select-v2
NPM package: a fork of [vue-single-select](https://github.com/robrogers3/vue-single-select/blob/master/src/VueSingleSelect.vue) by [Rob Rogers](https://github.com/robrogers3) to add extra features.
Compatible with Vue.js.

## FEATURES
Original features: 
- Selection of an item in a list, with v-model value
- Visual customisation
- Search based on any match anywhere

Added features in this fork version:
- Switched to search based on first letters match
- Navigating with tab button
- Open dropdown with down arrow
- Clicking on component toggles dropdown
- Various navigation improvements and fixes


## INSTALLATION

```bash
npm i vue-single-select-v2
```

and add this to the *<script>* part of your Vue file

```js
import VueSingleSelectV2 from 'vue-single-select-v2';

export default {
  components: {
    VueSingleSelectV2
  }
}
```

## HOW TO USE

The instructions are exactly the same as the original, and the [Readme of the original](https://github.com/robrogers3/vue-single-select/blob/master/README.md) is already very detailed and reliable.
Example:
```html
<vue-single-select-v2
  :value="yourVariable"
  :options="['1', 'test 2', 'I', 'turned', 'myself', 'into', 'a', 'pickle', 'morty']"/>
```

### NEW FIELDS

New fields were added for speed and convenience purposes:

#### 1. componentHeight

Type: `String`\
Height of the component. **DO NOT FORGET TO INCLUDE THE UNIT!**
Examples: '40px', '4rem'

```html
<vue-single-select-v2
  :value="yourVariable"
  :options="['1', '2', '3']"
  :componentHeight="'50px'"
  />
```

#### 2. backgroundColor

Type: `String`\
Color inside the field
Examples: 'white', 'rgb(250, 250, 250)', '#fff'

```html
<vue-single-select-v2
  :value="yourVariable"
  :options="['1', '2', '3']"
  :backgroundColor="'rgb(250, 250, 250)'"
  />
```

#### 3. iconSize

Type: `String`\
Height of the arrow and the X button to the right of the field. Again, **DO NOT FORGET TO INCLUDE THE UNIT!**
Examples: '15px', '1.5rem'

```html
<vue-single-select-v2
  :value="yourVariable"
  :options="['1', '2', '3']"
  :iconSize="'20px'"
  />
```

## LICENSE

Copyright 2023 Ilan Azoulay

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), 
to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, 
and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, 
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.