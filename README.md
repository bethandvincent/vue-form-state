[![npm version](https://badge.fury.io/js/vue-form-state.svg)](https://badge.fury.io/js/vue-form-state)
[![Build Status](https://travis-ci.com/netsells/vue-form-state.svg?branch=master)](https://travis-ci.com/netsells/vue-form-state)

# Vue Form State

Handle asynchronous loading, error and result states based on the result of a
promise.

## Installation
```
yarn add @netsells/vue-form-state
```

```javascript
import Vue from 'vue';
import VueFormState from '@netsells/vue-form-state';

Vue.use(VueFormState);
```

### Options

You can pass the following options to change the way it functions

```javascript
Vue.use(VueFormState, {
    parseError(error) {
        return error.response.data.message;
    },

    name: 'handle-form-state',
});
```

#### parseError

Parses an error for every form (i.e. globally). Output is stored in `error`
(original error is in `rawError`)

#### parseResult

Parses a response for every form (i.e. globally). Output is stored in `result`
(original response is in `rawResult`)

#### name

Change the name of the component (`form-state` by default)

## Usage

In your template:

```html
    <form-state :submit="submitForm" class="contact-form">
        <template
            v-slot:default="{
                submit,
                error,
                loading,
                result,
            }"
        >
            <form @submit.prevent="submit">
                <!-- your form -->
            </form>
        </template>
    </form-state>
```

Note that the `submit` callback is a prop on the `form-state` component. This is
so it has access to the return value (your promise).

In your methods:

```javascript
methods: {
    submitForm() {
        return fetch(url, {
            method: 'POST',
            body: JSON.stringify(this.formData),
        });
    }
}
```

The result of this promise will be set to `rawResult` in the slot. If it errors,
the error will be set to the `rawError` scoped slot. If you have supplied either
a `parseResult` or `parseError` optional functional, the result of these will be
available as `result` and `error` respectively.
