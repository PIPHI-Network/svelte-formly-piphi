<p align="center">
  <img width="100%" height="300" src="./logo.png" alt="Svelte Formly" />
</p>

# Svelte Formly

by [@kamalkech](https://github.com/kamalkech)

[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com) [![CircleCI](https://circleci.com/gh/beyonk-adventures/svelte-component-livereload-template.svg?style=shield)](https://circleci.com/gh/beyonk-adventures/svelte-component-livereload-template) [![svelte-v3](https://img.shields.io/badge/svelte-v3-blueviolet.svg)](https://svelte.dev)

## Features

- ⚡️ Generate dynamic and reactive forms.
- 😍 Easy to extend with custom field type, custom validation.
- ✔️ Compatible with Svelte, Svetelkit, Sapper, Routify

## Documentation

[Link Documentation](https://documentation-svelte-formly.vercel.app/)

## Installation

npm i svelte-formly

## Usage for svelte & sveltekit

```svelte
<script>
  import { Field } from "svelte-formly";

  const form_name = 'my_form';
  const fields = [
    {
      type: 'input',
      name: 'color',
      attributes: {
        type: 'color',
        label: 'Color Form',
        id: 'color',
        classes: ['class-field-color']
      }
    },
    {
      type: 'input',
      name: 'firstname',
      value: '',
      attributes: {
        type: 'text',
        label: 'Username',
        id: 'firstname',
        classes: ['form-control'],
        placeholder: 'Tap your first name'
      },
      rules: ['required', 'min:6'],
      messages: {
        required: 'Firstname field is required!',
        min: 'First name field must have more that 6 caracters!'
      }
    },
    {
      prefix: {
      	classes: ['custom-form-group']
      },
      type: 'input',
      name: 'lastname',
      value: '',
      attributes: {
      	type: 'text',
      	id: 'lastname',
      	placeholder: 'Tap your lastname',
      	classes: ['form-control']
      },
      description: {
      	classes: ['custom-class-desc'],
      	text: 'Custom text for description'
      }
    },
    {
      type: 'input',
      name: 'email',
      value: '',
      attributes: {
      	type: 'email',
      	id: 'email',
      	placeholder: 'Tap your email'
      },
      rules: ['required', 'email']
    },
    {
      type: 'radio',
      name: 'gender',
      extra: {
      	items: [
          {
            id: 'female',
            value: 'female',
            title: 'Female'
          },
          {
            id: 'male',
            value: 'male',
            title: 'Male'
          }
      	]
      }
    },
    {
      type: 'select',
      name: 'city',
      value: 1,
      attributes: {
      	id: 'city',
      	label: 'City'
      },
      rules: ['required'],
      extra: {
      	options: [
          {
            value: null,
            title: 'All'
          },
          {
            value: 1,
            title: 'Agadir'
          },
          {
            value: 2,
            title: 'Casablanca'
          }
      	]
      }
    }
  ];

  let message = '';
  let data = {};
  let color = '#ff3e00';

  function getValuesForm(event) {
    data = event.detail.data;
  }

  function onSubmit() {
    const { values } = data;
    if (data.valid) {
      color = values.color ? values.color : color;
      message = 'Congratulation! now your form is valid';
    } else {
      color = 'red';
      message = 'Your form is not valid!';
    }
  }
</script>
```

```svelte
<style>
  * {
    color: var(--theme-color);
  }
  .custom-form :global(.form-group) {
    padding: 10px;
    border: solid 1px var(--theme-color);
    margin-bottom: 10px;
  }
  .custom-form :global(.custom-form-group) {
    padding: 10px;
    background: var(--theme-color);
    color: white;
    margin-bottom: 10px;
  }
  .custom-form :global(.class-description) {
    color: var(--theme-color);
  }
</style>
```

```svelte
<h1 style="--theme-color: {color}">Svelte Formly</h1>
<h3>{message}</h3>
<form
  on:submit|preventDefault="{onSubmit}"
  class="custom-form"
  style="--theme-color: {color}"
>
  <Field {fields} name={form_name} on:Values={getValuesForm} />
  <button class="btn btn-primary" type="submit">Submit</button>
</form>
```

<hr>

### Params

Inputs : text, password, email, number, tel

```svelte
<script>
  fields = [
    {
      type: "input", // required
      name: "namefield", // required
      value: "", // optional
      attributes: {
        type: "text", // default=text or change to password, email, number, tel, color
        id: "id-field", // required
        classes: [], // optional
        label: "", // optional
        placeholder: "", // optional
        min: null, // optional
        max: null, // optional
        disabled: false, // optional
        readonly: false, // optional
      },
      extra: {}, // optional
      rules: [], // optional
      preprocess: (field, fields, values) => { // Hook to alter current field
        return field
      }
    }
  ]
</script>
```

Textarea

```svelte
<script>
  fields = [
    {
      type: "textarea", // required
      name: "name-field", // required
      value: "", // optional
      attributes: {
        id: "id-field", // required
        class: "", // optional
        label: "", // optional
        disabled: false, // optional
        readonly: false, // optional
        rows: null, // optional
        cols: null, // optional
      }
      extra: {}, // optional
      rules: [], // optional
      preprocess: (field, fields, values) => { // Hook to alter current field
        return field
      }
    }
  ]
</script>
```

Select

```svelte
<script>
  fields = [
    {
      type: "select", // required
      name: "name-field", // required
      attributes: {
        id: "id-field", // required
        classes: [], // optional
        label: "", // optional
        disabled: false, // optional
      },
      extra: {
        options: [
          {
            value: 1,
            title: 'option 1'
          },
          {
            value: 2,
            title: 'option 2'
          }
        ],
      }, // optional
      rules: [], // optional
      preprocess: (field, fields, values) => { // Hook to alter current field
        return field
      }
    }
  ]
</script>
```

Checkbox

```svelte
<script>
  fields = [
    {
      type: "checkbox", // required
      name: "name-field", // required
      attributes: {
        id: "id-field", // required
        classes: [], // optional
        label: "", // optional
      },
      extra: {
        items: [
          {
            value: 1,
            name: 'checkbox-1',
            title: 'checkbox 1'
          },
          {
            value: 2,
            name: 'checkbox-2',
            title: 'checkbox 2'
          }
        ],
      },
      rules: [], // optional
      preprocess: (field, fields, values) => { // Hook to alter current field
        return field
      }
    }
  ]
</script>
```

Radio

```svelte
<script>
  fields = [
    {
      type: "radio", // required
      name: "name-field", // required
      attributes: {
        id: "id-field", // required
        classes: [], // optional
        label: "", // optional
      },
      extra: {
        items: [
          {
            id: 'radio-1',
            value: 1,
            title: 'radio 1'
          },
          {
            id: 'radio-2',
            value: 2,
            title: 'radio 2'
          }
        ],
      },
      rules: [], // optional
      preprocess: (field, fields, values) => { // Hook to alter current field
        return field
      }
    }
  ]
</script>
```

Color

```svelte
<script>
  fields = [
    {
      type: 'input', // required
      name: 'name field', // required
      value: '#ff3e00', // optional
      attributes: {
        type: 'color', // optional
        id: 'id-field', // required
        classes: [], // optional
        label: 'Color', // optional
        disabled: false, // optional
      },
      rules: [], // optional
      preprocess: (field, fields, values) => {
        // Hook to alter current field
        return field;
      },
    },
  ]
</script>
```

Range

```svelte
<script>
  fields = [
    {
      type: 'input', // required
      name: 'name field', // required
      attributes: {
        type: 'range', // optional
        id: 'id-field', // required
        classes: [], // optional
        label: '', // optional
        min: 10, // required
        max: 100, // required
        step: 10, // required
      },
      rules: [], // optional
      preprocess: (field, fields, values) => {
        // Hook to alter current field
        return field;
      },
    }
  ]
</script>
```

<hr>

Autocomplete

```svelte
<script>
  fields = [
    {
      type: 'autocomplete', // required
      name: 'name field', // required
      attributes: {
        id: 'id-field', // optional
      },
      extra: {
        multiple: true, // optional
        loadItemes: [
          // list items with id and title attributes.
          {
            value: 1,
            title: 'item 1',
          },
          {
            value: 2,
            title: 'item 2',
          },
          {
            value: 3,
            title: 'item 3',
          },
          {
            value: 4,
            title: 'item 4',
          },
        ],
      },
      rules: [], // optional
      preprocess: (field, fields, values) => {
        // Hook to alter current field
        return field;
      },
    }
  ]
</script>
```

<hr>

File

```svelte
<script>
  fields = [
    {
      type: 'file',
      name: 'name-file',
      attributes: {
        id: 'id-field', // optional
        classes: [], // optional
        label: '', // optional
      },
      extra: {
        multiple: true, // optional, default=false
      },
      rules: ['file'],
      file: {
        types: 'jpg,gif,png',
        maxsize: 5, // 5MB
      },
    },
  ]
</script>
```

<hr>

### Validation

List rules to validate form.

```svelte
<script>
  const fields = [
    {
      ...,
      rules: [
        'required',
        'min:number',
        'max:number',
        'between:number:number',
        'equal:number',
        'email',
        'url'
        'file'
      ]
    }
  ];
</script>
```

Validation with custom rule

```svelte
<script>
  import { get } from "svelte/store";
  import { Field, values_form } from "svelte-formly";

  const fields = [
    {
      type: "text",
      name: "firstname",
      attributes: {
        id: "firstname",
      },
      rules: ["required"]
    },
    {
      type: "text",
      name: "lastname",
      attributes: {
        id: "lastname",
      },
      rules: ["required", notEqual, customRule2],
      messages: {
        notEqual: "Last name not equal to First name", // Custom message error, property name must equal to function name.
        customRule2: 'foo bar'
      }
    }
  ];

  // Custom rule to force field
  function notEqual() {
    const values = get(values_form).values;
    if (values.firstname === values.lastname) {
      return false;
    }
    return true;
  }

  function customRule2() {
    // ...others conditions.
  }
</script>
```
