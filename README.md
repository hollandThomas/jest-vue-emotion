# jest-vue-emotion

> Jest testing utilities for vue-emotion

# Why

Adding snapshot tests with Jest is a great way to help avoid unintended changes to your app's UI.

By diffing the serialized value of your Vue tree Jest can show you what changed in your app and allow you to fix it or update the snapshot.

By default snapshots with vue-emotion show generated class names. Adding jest-vue-emotion allows you to output the actual styles being applied.

# Installation

```bash
npm install --save-dev jest-vue-emotion
```

or

```bash
yarn add jest-vue-emotion
```

# Snapshot Serializer

The easiest way to test Vue components with vue-emotion is with the snapshot serializer. You can register the serializer via the `snapshotSerializers` configuration property in your jest configuration like so:

```js
// jest.config.js
module.exports = {
  // ... other config
  snapshotSerializers: ['<rootDir>/node_modules/jest-vue-emotion']
}
```


### Writing a test

Writing a test with `jest-vue-emotion` involves creating a snapshot from the `@vue/test-utils`'s resulting `wrapper`.

```js
import Vue from 'vue';
import styled from 'vue-emotion';
import { mount } from '@vue/test-utils';

const Greeting = styled('h1')`
  font-size: 42px;
  color: cornflowerblue;
`;

const StyledComponent = Vue.component('StyledComponent', {
  components: {
    Greeting,
  },
  template: '<Greeting>Hello, world!</Greeting>',
});

describe('StyledComponent', () => {
  it('renders with correct styles', () => {
    const wrapper = mount(StyledComponent);
    expect(wrapper).toMatchSnapshot();
  });
});

```

It'll create a snapshot that looks like this.

```jsx
// Jest Snapshot v1, https://goo.gl/fbAQLP

exports[`StyledComponent renders with correct styles 1`] = `
.css-1jiuwsj-Greeting {
  font-size: 42px;
  color: cornflowerblue;
}

<h1 class="css-1jiuwsj-Greeting eyqz2qb0">Hello, world!</h1>
`;
```

When the styles of a component change, the snapshot will fail and you'll be able to update the snapshot or fix the component.