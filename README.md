# JSON Schema for Humans

Quickly generate a beautiful HTML static page documenting a JSON schema

[Documentation (with visual examples)](https://coveooss.github.io/json-schema-for-humans)

## Installation
```
pip install json-schema-for-humans
```

## Usage

```
generate-schema-doc [OPTIONS] SCHEMA_FILE [RESULT_FILE]
```

`SCHEMA_FILE` must be a valid JSON Schema

The default value for `RESULT_FILE` is `schema_doc.html`

## Options

### --expand-buttons
Off by default

Add an `Expand all` and a `Collapse all` button at the top of the generated documentation

### --minify/--no-minify
On by default

Minify the output HTML document

### --deprecated-from-description
Off by default

Mark a property as deprecated (with a big red badge) if the description contains the string `[Deprecated`

### --default-from-description
Off by default

Extract the default value of a property from the description like this: ``[Default `the_default_value`]``

The default value from the "default" attribute will be used in priority

### --copy-css/--no-copy-css
On by default

Copy `schema_doc.css` to the same directory as `RESULT_FILE`.

### --copy-js/--no-copy-js
On by default.

Copy `schema_doc.min.js` to the same directory as `RESULT_FILE`.

This file contains the logic for the anchor links

## What's supported

See the excellent [Understanding JSON Schema](https://json-schema.org/understanding-json-schema/index.html) to understand what are those checks

The following are supported:
- Types
- Regular expressions
- Numeric types multiples and range
- Constant and enumerated values
- Required properties
- Default values
- Array `minItems`, `maxItems`, `uniqueItems`, `items` (schema that must apply to all of the array items), and `contains`
- Combining schema with `oneOf`, `allOf`, `anyOf`, and `not`

These are **not** supported at the moment (PRs welcome!):
- String length and format
- Property names, size, and pattern
- Array items at specific index (for example, first item must be a string and second must be an integer)
- Property dependencies
- Examples
- Media
- Conditional subschemas

References from inside a schema and to schemas in other files are supported (for example `{ $ref: "#/definitions/something" }` will be replaced by the 
content of `schema["definitions"]["something"]`).

## Anchor links
Clicking on a property or tab in the output documentation will set the hash of the current window. Opening this anchor link will expand all needed properties and scroll to this section of the schema. Useful for pointing your users to a specific setting.

For this feature to work, you need to include the Javascript file (`schema_doc.js`) that is automatically copied next to the output HTML file (`schema_doc.html` by default).

## Development

### Testing
Just run tox

`tox`

### Modifying Javascript
`schema_doc.js` is not minified automatically, you are responsible for doing it yourself

### Generating doc
The documentation is generated using jekyll and hosted on GitHub Pages

- Change your current working directory to `docs`
- Run ``python generate_examples.py``. This will get all examples from `tests/cases`, render the resulting HTML and
 include it in the appropriate folders in the jekyll site.
- If you have added an example, add the file name (without `.json`), the display name and description in `_data/examples.yaml`