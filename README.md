<img src="./src/icon.svg" alt="Craft Api2Pdf icon" width="150" />

# Api2Pdf plugin for Craft CMS 3.x

Generate PDFs easily, using [Api2Pdf.com](https://www.api2pdf.com)

## Requirements

This plugin requires Craft CMS 3.1.x or later.

## Installation

To install the plugin, require the plugin using Composer:

```sh
composer require kennethormandy/craft-api2pdf
```

Then, in the Craft CMS Control Panel, go to Settings → Plugins, and click the “Install” button for Craft Api2Pdf. Or run:

```sh
./craft install/plugin api2pdf
```

## Settings

The only setting required to run the plugin is an API key from Api2Pdf, which can (and probably should) be set to an [environment variable](https://docs.craftcms.com/v3/config/environments.html):

![Craft Api2Pdf settings panel in the Craft CMS admin area](https://user-images.githubusercontent.com/1581276/72213557-ebe31f00-34a5-11ea-8d49-c8b2f0327828.png)

## Actions

- [`api2pdf/pdf/generate-from-url`](#action-generate-from-url)
- [`api2pdf/pdf/generate-from-html`](#action-generate-from-html)

## Twig

- [`craft.api2pdf.generateFromUrl`](#twig-generatefromurl-function)
- [`craft.api2pdf.generateFromHtml`](#twig-generatefromhtml-function)

## Options

<table>
<thead>
  <tr>
    <th>Option</th>
    <th>Type</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>filename</code></td>
    <td>String</td>
    <td></td>
  </tr>
  <tr>
    <td><code>redirect</code></td>
    <td>Boolean</td>
    <td>Redirect directly to the PDF URL</td>
  </tr>
</tbody>
</table>

- `filename`
- `redirect`

All [advanced options for Headless Chrome](https://www.api2pdf.com/documentation/advanced-options-headless-chrome/) to pass along to Api2Pdf are also supported.

## Examples

### Action `generate-from-url`

Get the JSON response from Api2Pdf:

```html
<form method="post" action="" accept-charset="UTF-8">
  <input
    type="hidden"
    name="action"
    value="api2pdf/pdf/generate-from-url"
  />
  <input type="hidden" name="url" value="https://example.com" />
  {{ csrfInput() }}
  <input type="submit" value="Generate PDF from URL" />
</form>
```

Redirect directly to the PDF url:

```html
<form method="post" action="" accept-charset="UTF-8">
  
  <input
    type="hidden"
    name="action"
    value="api2pdf/pdf/generate-from-url"
  />

  <!-- Set the URL (your site or any public URL) to use -->
  <input type="hidden" name="url" value="https://example.com" />

  <!-- Redirect to the PDF URL -->
  <input type="hidden" name="options[redirect]" value="1" />

  {{ csrfInput() }}
  <input type="submit" value="Generate PDF from URL with a redirect" />
</form>
```

### Action `generate-from-url`

Redirect directly to the PDF made using an HTML string:

```html
<form method="post" action="" accept-charset="UTF-8">

  <input
    type="hidden"
    name="action"
    value="api2pdf/pdf/generate-from-html"
  />

  <!-- Set the custom HTML string -->
  <input type="hidden" name="html" value="<p>Hello from action with HTML</p>" />

  <!-- Redirect to the PDF URL -->
  <input type="hidden" name="options[redirect]" value="1" />

  {{ csrfInput() }}
  <input type="submit" value="Generate with redirect from HTML" />
</form>
```

Offer an editable filename:

```html
<form method="post" action="" accept-charset="UTF-8">

  <input
    type="hidden"
    name="action"
    value="api2pdf/pdf/generate-from-html"
  />

  <!-- Set the custom HTML string -->
  <input type="hidden" name="html" value="<p>Hello</p>" />

  <!-- Set the filename to the value of the form input -->
  <input type="text" name="options[filename]" value="test.pdf" />

  {{ csrfInput() }}
  <input type="submit" value="Generate from HTML" />
</form>
```

### Twig generateFromUrl function

```twig
{% set result = craft.api2pdf.generateFromUrl('https://example.com') %}

{% if result and result.success %}
  {{ result.url }}
{% endif %}
```

### Twig generateFromHtml function

```twig
{% set result = craft.api2pdf.generateFromHtml('<h1>Hello</h1>') %}

{% if result and result.success %}
  {{ result.url }}
{% endif %}
```

Slightly more detailed example:

```twig
{% set redirect = false %}
{% set options = {
  redirect: true,
  filename: "test.pdf"
} %}
{% set result = craft.api2pdf.generateFromHtml('<h1>Hello</h1>', options) %}

{% if result and result.success %}

  {# Display the URL #}
  <p>{{ result.pdf }}</p>

  {# The other pieces of metadata available #}
  <ul>
    <li>{{ result.mbIn|round }}mb</li>
    <li>{{ result.mbOut|round }}mb</li>
    <li>US${{ result.cost|round }}</li>
    <li>{{ result.responseId }}</li>
  </ul>
  
{% else %}
  <p>{{ result.error }}</p>
{% endif %}
```

## Notes

- This plugin only supports Headless Chrome for PDF generation. I plan on adding support for `merge`, but not any other endpoints. If you are interested in adding support for another endpoint, I’d be open to a Pull Request.
- This plugin is build for v1 of the Api2Pdf API, but support for the [v2 endpoint](https://www.api2pdf.com/api2pdf-launches-v2-in-beta/) may be added when it’s out of beta, the Api2Pdf client libraries are also updated

<!--

## Templates

In progress. Add your own templates in `templates/pdf` (right now, the specific template is hard-coded for me, but you will be able to pass along a file name).

-->

## License

[The MIT License (MIT)](./LICENSE.md)

Copyright © 2019–2020 [Kenneth Ormandy Inc.](https://kennethormandy.com)
