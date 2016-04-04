#LibGDX Markup Language
Templates for LibGDX Scene2D with HTML-like syntax and FreeMarker-inspired macros.

##Examples
See [gdx-lml-tests](https://github.com/czyzby/gdx-lml/tree/master/examples/gdx-lml-tests) for example uses of all available tags and macros.

Check it out [on-line](http://czyzby.github.io/gdx-lml/lml).

###Documentation
See [LibGDX forum thread](http://www.badlogicgames.com/forum/viewtopic.php?f=17&t=18843), [example project](https://github.com/czyzby/gdx-lml/tree/master/examples/gdx-lml-tests), [tutorial](https://github.com/czyzby/gdx-lml/wiki/Tutorial), [DTD file](https://github.com/czyzby/gdx-lml/tree/master/lml/dtd) and [old syntax page (work in progress)](https://github.com/czyzby/gdx-lml/wiki/Syntax).

##Extensions
[VisUI](https://github.com/kotcrab/VisEditor/wiki/VisUI) syntax support is available through [gdx-lml-vis library](https://github.com/czyzby/gdx-lml/tree/master/lml-vis). Additionally to replacing standard Scene2D actors with improved VisUI widgets, it offers full support for other VisUI features - including color picker, file chooser, other new widgets, listeners and validators.

Check it out [on-line](http://czyzby.github.io/gdx-lml/lml-vis).

## Dependencies

`gdx-lml` is available through the official project creator tool: `gdx-setup` (in additional extensions). However, its version might not be up to date.

To import LML with `Gradle`, add this dependency to your core project:
```
        compile "com.github.czyzby:gdx-lml:$libVersion.$gdxVersion"
```

`$libVersion` is the current version of the library, usually following `MAJOR.MINOR` schema. `$gdxVersion` is the LibGDX version used to build (and required by) the library. You can check the current library version [here](http://search.maven.org/#search|ga|1|g%3A%22com.github.czyzby%22) - or you can use the [snapshots](https://oss.sonatype.org/content/repositories/snapshots/com/github/czyzby/).

If you want to use LML with GWT, you have to add this module to your `GdxDefinition`:
```
        <inherits name='com.github.czyzby.lml.GdxLml' />
```

## What's new

1.5 -> 1.6

- Removed `/*` alias for comment macro. Since `DTD` creator was added to LML, now it is possible to create templates that are somewhat-valid `XML` files. `/*` was the only default tag that used forbidden `XML` characters.
- New actor: `AnimatedImage`. Extends the regular `Image`; manages an array of drawables updated on `act` method. Allows to display a set of images in the specified way. Available through `animate, animation, animatedImage` tags.

1.5.1.8.0 -> 1.5.1.9.2

- Now when parsing a string value, a single character will not be treated as bundle line, preference or action (etc.) - even if it matches `@`, `$` or any other functional character. So, `$` will be parsed to `"$"`, but `$$$` will still look for an action (mapped to `$$` key). This is a simple convenience for printing a single character - these cannot be properly used as property names or bundle lines (and so on) anyway, as at least 2 characters are required. Note that if you want to use multiple restricted characters, you can always use i18n bundles.
- *DTD* file generator. Now you can generate a *DTD* file based on your customized `LmlParser` with `Dtd` class.

1.4 -> 1.5

- `argument` macro added. Contrary to `assign` macro, this macro evaluates passed arguments. For example, `assign` macro would assign `@someBundleLine` to argument name, while `argument` value would convert it to `Actual bundle line value in .properties file.` and assign it to the argument. See `ArgumentLmlMacroTag` for more informations.
- Ranges now accept bundle lines, preferences and methods. For example, now you can customize range size with i18n bundle file: `range[@start,@end]`, provided that `.properties` file has numeric `start` and `end` lines.
- Equations (available through `if`, `while` and `calculate` macros) now parse bundle lines and preferences. For example, `<@if @someLine < 20>` will check if bundle line mapped to `someLine` is shorter than 20 characters in the current locale.
- Equation marker. Now equations can be used pretty much anywhere, using mechanism similar to arguments. Normally, you insert parser arguments `{likeThat}` - this will look for an argument named `likeThat` and replace the braces block with its value (or `null`). To use equation instead, add `=` character at the beginning. For example, `{=3+5}` will replace the block with `8`. Bundle lines, preferences and methods are also supported by these equations. Equation marker aims to be a simplified alternative to `calculate` macro. As usual: to change equation marker, extend `DefaultLmlSyntax` class and override appropriate method. See new equations example in `gdx-lml-tests`.

1.3 -> 1.4

- `PooledList` is now used instead of `LinkedList`, which should slightly speed up the parsing (as in: limit its garbage collection) thanks to cached nodes and iterators.

### Archive
Older change logs are available in `CHANGES.md` file.