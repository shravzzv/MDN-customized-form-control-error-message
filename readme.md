# Customized error message using the Constraint Validation API

The automated error messages for form controls have two drawbacks:

- There is no standard way to change their look and feel with CSS.
- They depend on the browser locale, which means that you can have a page in one language but an error message displayed in another language.

Customizing these error messages is one of the most common use cases of [The Constraint Validation API](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#the_constraint_validation_api).

## Simple customized error message

```html
<input type="email" id="mail" name="mail" />
```

```js
const email = document.getElementById('mail')

email.addEventListener('input', (event) => {
  if (email.validity.typeMismatch) {
    email.setCustomValidity('I am expecting an email address!')
  } else {
    email.setCustomValidity('')
  }
})
```

Inside the contained code, we check whether the email input's `validity.typeMismatch` property returns `true`, meaning that the contained value **doesn't match** the pattern for a well-formed email address.

If so, we call the `setCustomValidity()` method with a custom message. This renders the input invalid, so that when you try to submit the form, submission fails and the custom error message is displayed.

If the `validity.typeMismatch` property returns `false`, we call the `setCustomValidity()` method with an empty string. This renders the input valid, so the form will submit.
