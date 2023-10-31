# Drupal Module Email Obfuscator

The Drupal Email Obfuscator Module uses a middleware get rendered content from each request. The content is searched for
emails with regexs. The emails are obfuscated depending on where the text is found.

## Obfuscations

### Emails in a Mailto-Link

Example: `<a href="mailto:test@email.com">`

- The email string excluding `mailto:` is reversed
- An onclick is added that re-reverses the email after the `mailto:`

### All other Emails

Example: `<a>test@email.com</a>`

- A span with `display:none` containing a text with delimiters that are invalid email characters is added in the middle
  of the email

## Exclusions

- Everything in the backoffice (admin pages)
- Emails in HTML-attributes (placeholder for input fields)
- Content in routes that are whitelisted (see below)

### Whitelisting Routes

- Define whitelisted (excluded) routes in settings.php
   ```php
   $settings['email_obfuscator'] = [
     'route_whitelist' => [
       'rest.api_layout_footer.GET',
       'editor.link_dialog'
     ]
   ];
   ```
- **IMPORTANT:** If you are using CKEditor 4 you should whitelist the route `editor.link_dialog` to avoid
  obfuscating the email in the CKEditor link dialog.
