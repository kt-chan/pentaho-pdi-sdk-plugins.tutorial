## Message Bundles

PDI uses property files for internationalization. Property files reside in the `messages` sub-package in the plugin `jar` file. Each property file is specific to a locale. Property files contain translations for message keys that are used in the source code. A messages sub-package containing locale-specific translations is called a message bundle.

Consider the package layout of the sample job entry plugin project. It contains its main Java class in the `org.pentaho.di.sdk.samples.jobentries.demo` package, and there is a message bundle containing the localized strings for the `en_US` locale.  

Additional property files can be added using the naming pattern `messages_<locale>.properties`. PDI core steps and job entries usually come with several localizations. See the [Shell job entry messages package][Shell Messages Package] for an example of more complete i18n.

## Resolving Localized Strings

The key to resolving localized strings is to use the `getString()` methods of `org.pentaho.di.i18n.BaseMessages`. PDI follows conventions when using this class, which enables easy integration with the [PDI translator tool][PDI translator tool].

All PDI plugin classes that use localization declare a `private static Class<?> PKG` field, and assign a class that lives one package-level above the message bundle package. This is often the main class of the plugin.

With the PKG field defined, the plugin then resolves its localized strings with a call to `BaseMessages.getString(PKG, “localization key”, ... optional_parameters)`. The first argument helps PDI finding the correct message bundle, the second argument is the key to localize, and the optional parameters are injected into the localized string following the Java [Message Format][Message Format] conventions.

## Common Localization Strings

Some strings are commonly used, and have been pulled together into a common message bundle in `org.pentaho.di.i18n.messages`. Whenever `BaseMessages` cannot find the key in the specified message bundle, PDI looks for the key in the common message bundle.

Example

For an example, check the sample Job Entry plugin project, which uses this technique for localized string resolution in its dialog class.

[Shell Messages Package]: https://github.com/pentaho/pentaho-kettle/tree/master/engine/src/org/pentaho/di/job/entries/shell/messages
[PDI translator tool]: http://wiki.pentaho.com/display/EAI/Kettle+4+and+the+art+of+internationalization
[Message Format]: http://docs.oracle.com/javase/6/docs/api/java/text/MessageFormat.html