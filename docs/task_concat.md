[Grunt homepage](https://github.com/cowboy/grunt) | [Documentation table of contents](toc.md)

# concat

The `concat` task is a [basic task](tasks_creating.md), and very simple. It concatenates one or more files (and/or [directives](helpers_directives.md) output, like `<banner>`) into an output file.

For more information on general configuration options, see the [configuring grunt](configuring.md) page.

## Helpers

A generic `concat` helper is available for use in any other task where file and/or directive concatenation might be useful. For example:

```javascript
var fooPlusBar = task.helper('concat', ['foo.txt', 'bar.txt']);
```

## Examples

In this example, `grunt concat` will simply concatenate three source files, in order, writing the output to `dist/built.js`.

```javascript
/*global config:true, task:true*/
config.init({
  concat: {
    'dist/built.js': ['src/intro.js', 'src/project.js', 'src/outro.js']
  }
});
```

In this example, `grunt concat` will first strip any pre-existing banner comment from the `src/project.js` file, then concatenate that with a newly-generated banner comment, writing the output to `dist/built.js`.

This generated banner will be the contents of the `meta.banner` mustache template string interpolated (in this case) with values imported from the `package.json` file (which are available via the `pkg` config property).

```javascript
/*global config:true, task:true*/
config.init({
  pkg: '<json:package.json>',
  meta: {
    banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - <%= template.today("m/d/yyyy") %> /*'
  },
  concat: {
    'dist/built.js': ['<banner>', '<file_strip_banner:src/project.js>']
  }
});
```

In this example, `grunt concat` will build two separate files. One "basic" version, with the main file essentially just copied to `dist/basic.js`, and another "with_extras" concatenated version written to `dist/with_extras.js`.

While each concat target can be built individually by running `grunt concat:dist/basic.js` or `grunt concat:dist/with_extras.js`, running `grunt concat` will build all concat targets. This is because concat is a [basic task](tasks_creating.md).

```javascript
/*global config:true, task:true*/
config.init({
  concat: {
    'dist/basic.js': ['src/main.js'],
    'dist/with_extras.js': ['src/main.js', 'src/extras.js']
  }
});
```