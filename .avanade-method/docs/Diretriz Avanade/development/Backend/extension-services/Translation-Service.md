# Translation Service

This service allows .resx web resources which are typically only used for the frontend to be also used in the backend.

# Typical Use Cases

- All kinds of text translation in the backend (it is also possible to get translations from Power Automate flows via Custom API or Commands)

# Methods

`string GetTextForCurrentUserLanguage(string key, params object[] inputArguments)`

Gets a translated text for the current user derived from the execution context. This only works in a plug-in.

`string GetText(string key, Guid userId, params object[] inputArguments)`

Gets the translated text for a given user using their current language.

`string GetText(string key, int languageCode, params object[] inputArguments)`

Gets the translated text for a given language code. This is useful if you for example want to translate a given text in the language of a customer account.

# Example

1. The .resx file must be available in the web resources folder and added to a solution.
2. The .resx file must include at minimum the keys and translation texts in the format `<root><data name="SomeKey"><value>Translated Text</value></data>...</root>`.
3. In any backend, use the service like `string translatedTextForSomeKey = _translationService.GetTextForCurrentUserLanguage("SomeKey");`