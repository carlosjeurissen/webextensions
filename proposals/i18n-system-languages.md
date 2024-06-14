# Proposal: i18n.getPreferredSystemLanguages() and i18n.getSystemUILanguage()

**Summary**

Allows getting the language(s)/locale(s) of your operating system as a [BCP47 language tag](https://www.rfc-editor.org/bcp/bcp47.html)

**Document Metadata**

**Author:** [carlosjeurissen](https://github.com/carlosjeurissen)

**Sponsoring Browser:** TBD

**Created:** 2024-03-18

**Related Issues:**
https://github.com/w3c/webextensions/issues/252
https://bugzilla.mozilla.org/show_bug.cgi?id=1888486

## Motivation

### Objective

Allow extensions to display UI in the user's system language(s), even when this is distinct from the browser's UI language (which is restricted to languages supported by the browser).

#### Use Cases

Having the original system language(s) is useful for language-related extensions and for extensions who want to provide translations more true to the system language. For example, to adapt to a specific sub-language, like Belgium-Dutch (nl-BE). While i18n.getUILanguage would return 'nl'.

### Known Consumers

Language-related extensions and extensions wanting to match the language of the operating system more closely independent of the language of the browser.

## Specification

### Schema

`i18n.getPreferredSystemLanguages()` would asynchronously return a list of [BCP47 language tag](https://www.rfc-editor.org/bcp/bcp47.html) like `i18n.getAcceptLanguages()` does right now.

```json
{
  "name": "getPreferredSystemLanguages",
  "type": "function",
  "description": "Gets the preferred languages of the operating system. This is different from the languages set in the browser; to get those, use $(ref:i18n.getAcceptLanguages).",
  "parameters": [],
  "returns_async": {
    "name": "callback",
    "parameters": [
      {"name": "languages", "type": "array", "items": {"$ref": "LanguageCode"}, "description": "Array of LanguageCode"}
    ]
  }
}

```

`i18n.getSystemUILanguage()` would synchronously return a [BCP47 language tag](https://www.rfc-editor.org/bcp/bcp47.html) like `i18n.getUILanguage()` does right now.

The returned language tag can be different from the first entry returned by getPreferredSystemLanguages as the operating system could not support all languages specified by the user for its user interface.

It would follow the following signature for [i18n.json](https://chromium.googlesource.com/chromium/src/+/4299ce68496b32ba309e2f012e0db5b4b8cd478a/extensions/common/api/i18n.json):

```json
{
  "name": "getSystemUILanguage",
  "type": "function",
  "nocompile": true,
  "description": "Gets the current UI language of the Operating System. This is different from $(ref:i18n.getUILanguage) which returns the UI language of the web browser.",
  "parameters": [],
  "returns": {
    "type": "string",
    "description": "A BCP47 language tag such as en-US or pt-BR."
  }
}
```

### New Permissions

As `browser.i18n.getUILanguage()` does not require a permission, browser.i18n.getSystemUILanguage and browser.i18n.getPreferredSystemLanguages also should not.

### Manifest File Changes

No new manifest fields

## Security and Privacy

### Exposed Sensitive Data

The language(s) of the system will be purposely exposed.

## Alternatives

### Existing Workarounds

Currently, if an extension wants to offer languages outside of what the current browser language uses, it must do so by presenting a user-facing options page. However, currently there is no way to reasonably detect a language code outside of the browser's UI language with existing APIs.

### Open Web API

To combat fingerprinting, this should not be an Open Web API, like browser.i18n.getUILanguage is not.