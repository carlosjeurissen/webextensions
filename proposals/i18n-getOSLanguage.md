# Proposal: i18n.getOSLanguage>

**Summary**

Allow to get the language of your operating system as an `IETF tag`

**Document Metadata**

**Author:** carlosjeurissen

**Created:** 2024-03-18

**Related Issues:** https://github.com/w3c/webextensions/issues/252

## Motivation

### Objective

As browsers often do not directly use the OS language but some reflection of it based on what languages are supported by the browser UI.

#### Use Cases

Having the original OS language is useful for language-related extensions and for extensions who want to provide translations more true to the OS language. For example, to adapt to a specific sub-language, like Belgium-Dutch (nl-BE). While i18n.getUILanguage would return 'nl'.

### Known Consumers

Language-related extensions and extensions wanting to match the language of the OS more closely independent of the langauge of the browser.

## Specification

### Schema

`i18n.getOSLanguage()` would synchronously return an IETF tag just like `i18n.getUILanguage()` does right now.

It would follow the following signature for [i18n.json](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/extensions/common/api/i18n.json):


```json
{
  "name": "getOSLanguage",
  "type": "function",
  "nocompile": true,
  "description": "Gets the UI language of the Operating System. This is different from $(ref:i18n.getUILanguage) which returns the UI language of the web browser.",
  "parameters": [],
  "returns": {
    "type": "string",
    "description": "An IETF BCP 47 language tag such as en-US or pt-BR."
  }
}
```

### New Permissions

As browser.i18n.getUILanguage does not require a permission. browser.i18n.getOSLanguage also should not.

### Manifest File Changes

No new manifest fields

## Security and Privacy

### Exposed Sensitive Data

The language of the OS will be purposely exposed.

## Alternatives

### Existing Workarounds

There is no current workaround.

### Open Web API

To combat fingerprinting, this should not be an Open Web API. Just like browser.i18n.getUILanguage is not.