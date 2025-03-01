---
title: Radio
---

# Radio

> Select an option from a set

---

<a href="https://github.com/oruga-ui/oruga/edit/develop/packages/docs/../oruga/src/components/radio/examples/Radio.md" class="docgen-edit-link">edit on github</a>

## Examples

### Base

::: demo

```html
<template>
  <section>
    <div class="block">
      <o-radio v-model="radio" name="name" native-value="Flint">
        Flint
      </o-radio>
      <o-radio v-model="radio" name="name" native-value="Silver">
        Silver
      </o-radio>
      <o-radio v-model="radio" name="name" native-value="Jack">
        Jack
      </o-radio>
      <o-radio v-model="radio" name="name" native-value="Vane" disabled>
        Vane
      </o-radio>
    </div>
    <p class="content">
      <b>Selection:</b>
      {{ radio }}
    </p>
  </section>
</template>

<script>
  export default {
    data() {
      return {
        radio: 'Jack'
      }
    }
  }
</script>
```

:::

### Variants

::: demo

```html
<template>
  <section>
    <div class="field">
      <o-radio v-model="radio" native-value="default">
        Default
      </o-radio>
    </div>
    <div class="field">
      <o-radio v-model="radio" native-value="info" variant="info">
        Info
      </o-radio>
    </div>
    <div class="field">
      <o-radio v-model="radio" native-value="success" variant="success">
        Success
      </o-radio>
    </div>
    <div class="field">
      <o-radio v-model="radio" native-value="danger" variant="danger">
        Danger
      </o-radio>
    </div>
    <div class="field">
      <o-radio v-model="radio" native-value="warning" variant="warning">
        Warning
      </o-radio>
    </div>
  </section>
</template>

<script>
  export default {
    data() {
      return {
        radio: 'default'
      }
    }
  }
</script>
```

:::

## Class props

📄 [Full scss file](https://github.com/oruga-ui/oruga/blob/master/packages/oruga/src/scss/components/_radio.scss)

<br />
<template>
  <div class="field">
    <doc-wrapper>
      <template v-slot:default="s">
        <o-radio v-model="s.checkbox" native-value="default" v-bind="s">Radio</o-radio>
      </template>
    </doc-wrapper>
    <inspector :inspectData="inspectData"></inspector>
  </div>
</template>

<script>

export default {
  data() {
    return {
      inspectData: [
        {
          class: 'rootClass',
          description : 'Class of the root element'
        },
        {
          class: 'labelClass',
          description : 'Class of the radio label'
        },
        {
          class: 'checkClass',
          description : 'Class of the native radio element'
        },
        {
          class: 'checkedClass',
          description : 'Class of the root element when checked',
          action: (cmp) => {
            cmp.data.checkbox = 'default';
          }
        },
        {
          class: 'checkCheckedClass',
          description : 'Class of the native radio element when checked',
          action: (cmp) => {
            cmp.data.checkbox = 'default';
          }
        },
        {
          class: 'sizeClass',
          description : 'Class of the radio size',
          properties: ["size"],
          suffixes: ['small', 'medium', 'large'],
          action: (cmp) => {
            cmp.data.size = 'large';
          }
        },
        {
          class: 'disabledClass',
          description : 'Class when radio is disabled',
          properties: ["disabled"],
          action: (cmp) => {
            cmp.data.disabled = true;
          }
        },
        {
          class: 'variantClass',
          description : 'Class of the radio variant',
          properties: ["variant"],
          suffixes: ['primary', 'info', 'warning', 'danger'],
          action: (cmp) => {
            cmp.data.variant = 'warning';
          }
        }
      ]
    }
  }
}
</script>

<br />
<br />

## Props

| Prop name   | Description                    | Type                           | Values                                                                          | Default |
| ----------- | ------------------------------ | ------------------------------ | ------------------------------------------------------------------------------- | ------- |
| disabled    | Same as native disabled        | boolean                        | -                                                                               |         |
| name        | Same as native name            | string                         | -                                                                               |         |
| nativeValue | Same as native value           | string\|number\|boolean\|array | -                                                                               |         |
| override    | Override classes               | boolean                        | -                                                                               | false   |
| required    |                                | boolean                        | -                                                                               |         |
| size        | Size of the control, optional  | string                         | `small`, `medium`, `large`                                                      |         |
| v-model     |                                | string\|number\|boolean\|array | -                                                                               |         |
| variant     | Color of the control, optional | string                         | `primary`, `info`, `success`, `warning`, `danger`, `and any other custom color` |         |

## Events

| Event name | Properties | Description |
| ---------- | ---------- | ----------- |
| input      |            |

## Slots

| Name    | Description | Bindings |
| ------- | ----------- | -------- |
| default |             |          |

## Style

| CSS Variable                             | SASS Variable                      | Default                 |
| ---------------------------------------- | ---------------------------------- | ----------------------- |
| --oruga-radio-active-background-color    | \$radio-active-background-color    | \$primary               |
| --oruga-radio-checked-box-shadow-length  | \$radio-checked-box-shadow-length  | 0 0 0.5em               |
| --oruga-radio-checked-box-shadow-opacity | \$radio-checked-box-shadow-opacity | 0.8                     |
| --oruga-radio-disabled-opacity           | \$radio-disabled-opacity           | \$base-disabled-opacity |
| --oruga-radio-label-padding              | \$radio-label-padding              | 0 0 0 .5em              |
| --oruga-radio-margin-sibiling            | \$radio-margin-sibiling            | 0.5em                   |
| --oruga-radio-size                       | \$radio-size                       | 1rem                    |
| --oruga-radio-line-height                | \$radio-line-height                | 1.25                    |
