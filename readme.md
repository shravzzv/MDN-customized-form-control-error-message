# Customized error message using the Constraint Validation API

[The Constraint Validation API](https://developer.mozilla.org/en-US/docs/Learn/Forms/Form_validation#the_constraint_validation_api) gives you a powerful tool to handle form validation, letting you have enormous control over the user interface above and beyond what you can do with HTML and CSS alone.

The automated error messages for form controls have two drawbacks:

- There is no standard way to change their look and feel with CSS.
- They depend on the browser locale, which means that you can have a page in one language but an error message displayed in another language.

Customizing these error messages is one of the most common use cases of The Constraint Validation API.

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

- Inside the contained code, we check whether the email input's `validity.typeMismatch` property returns `true`, meaning that the contained value **doesn't match** the pattern for a well-formed email address.

- If so, we call the `setCustomValidity()` method with a custom message. This renders the input invalid, so that when you try to submit the form, submission fails and the custom error message is displayed.

- If the `validity.typeMismatch` property returns `false`, we call the `setCustomValidity()` method with an empty string. This renders the input valid, so the form will submit.

## Advanced customized error message

```html
<form novalidate>
  <p>
    <label for="mail">
      <span>Please enter an email address:</span>
      <input type="email" id="mail" name="mail" required minlength="8" />
      <span class="error" aria-live="polite"></span>
    </label>
  </p>
  <button type="submit">Submit</button>
</form>
```

- The `form` uses the `novalidate` attribute to turn off the browser's automatic validation; this lets our script take control over validation.

```js
const form = document.querySelector('form')
const email = document.getElementById('mail')
const emailError = document.querySelector('#mail + span.error')

email.addEventListener('input', (event) => {
  if (email.validity.valid) {
    emailError.textContent = ''
    emailError.className = 'error'
  } else {
    showError()
  }
})

form.addEventListener('submit', (event) => {
  if (!email.validity.valid) {
    showError()
    event.preventDefault()
  }
})

function showError() {
  if (email.validity.valueMissing) {
    emailError.textContent = 'You need to enter an email address.'
  } else if (email.validity.typeMismatch) {
    emailError.textContent = 'Entered value needs to be an email address.'
  } else if (email.validity.tooShort) {
    emailError.textContent = `Email should be at least ${email.minLength} characters; you entered ${email.value.length}.`
  }
}
```

- Every time we change the value of the `input`, we check to see if it contains valid data.

  - If it has then we remove any error message being shown.
  - If the data is not valid, we run `showError()` to show the appropriate error.

- Every time we try to submit the form, we again check to see if the data is valid.

  - If so, we let the form submit.
  - If not, we run `showError()` to show the appropriate error, and stop the form submitting with `preventDefault()`.

- The `showError()` function uses various properties of the **input's validity object** to determine what the error is, and then displays an error message as appropriate.
