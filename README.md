# Superstatic Forms

A simple service for capturing submitted forms via email.

[![NPM Module](http://img.shields.io/npm/v/superstatic-forms.svg?style=flat)](https://npmjs.org/package/superstatic-forms)
[![Build Status](http://img.shields.io/travis/divshot/superstatic-forms.svg?style=flat)](https://travis-ci.org/divshot/superstatic-forms)

## Client Configuration

Superstatic Forms is configured by providing a JSON object with keys
named for a specific form and values as described below. For example,
your `superstatic.json` might have a section like this:

```json
{
  "forms": {
    "contact": {
      "to":"Company Contact <info@your-company.com>",
      "reply_to":"{{email}}",
      "subject":"Contact Received from {{name}}",
      "html":"<b>Name:</b> {{name}}",
      "text":"Name: {{name}}",
      "success":"/contact-received",
      "failure":"/contact-failure"
    },
    "beta": {
      "to":"beta@your-company.com",
      "subject":"Beta Signup",
      "text":"{{name}} signed up for the private beta."
    }
  }
}
```

This would allow a form with method `POST` and action `/__/forms/contact`
to submit a contact email, and action `/__/forms/beta` to submit a beta
signup email. For example:

```html
<form method="POST" action="/__/forms/contact">
  <label>Name:</label> <input name="name" type="text">
  <label>Email:</label> <input name="email" type="submit">
  <button type="submit">Contact Us</button>
</form>
```

### Configuration Options

* **to:** Email address of the recipient with optional name. This field **cannot be dynamic**.
* **reply_to:** Reply-to address for easy follow-up.
* **subject:** Subject of the email.
* **html:** (optional) HTML template for the email body.
* **text:** (optional) Plain text template for the email body.
* **success:** (optional) Redirect URL on successful submission.
* **failure:** (optional) Redirect URL on failure.

The `subject`, `reply_to`, `html`, and `text` fields are all rendered using Handlebars. If you don't supply `html` or `text` a simple list of the submitted form information will be added automatically.

**Note:** To prevent spam and other abuse, the `to` address is only configurable in `superstatic.json`. It is not templatable.

## Server-Side

You must provide server-side global configuration so that Superstatic
Forms is able to send email. This service uses [nodemailer](http://nodemailer.com)
in the background, so configuration is based on that.

```js
require('superstatic-forms')({
  from: "no-reply@your-company.com",
  transport: 'SMTP',
  options: {
    // ...
  }
});
```
