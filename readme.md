# Forko (an update to the original project, Gorko)

A tiny, Sass-powered, design-token led, utility class generator (with handy helpers) that helps you to power your frontends with a single source of truth.

## Table of contents

- [Forko](#forko)
  - [Getting started](#getting-started)
  - [Configuration](#configuration)
    - [Base size (optional)](#base-size--optional-)
    - [Size scale (optional)](#size-scale--optional-)
    - [Colors (optional)](#colors--optional-)
    - [Forko config (required)](#forko-config--required-)
    - [Breakpoints](#breakpoints)
  - [Utility Class Generator](#utility-class-generator)
    - [Example outputs](#example-outputs)
  - [Generating Utility Classes On Demand](#generating-utility-classes-on-demand)
  - [Using CSS Custom Properties](#using-css-custom-properties)
    - [Using themes](#using-themes)
  - [Sass functions](#sass-functions)
    - [Get color](#get-color)
      - [Example](#example)
    - [Get utility value](#get-utility-value)
      - [Example](#example-1)
    - [Get size](#get-size)
      - [Example](#example-2)
  - [Sass mixins](#sass-mixins)
    - [Apply utility](#apply-utility)
      - [Example](#example-3)
    - [Media query](#media-query)
      - [Example](#example-4)
  - [Contributing](#contributing)
  - [Pull Request Process](#pull-request-process)
  - [Code of Conduct](#code-of-conduct)
    - [Our Pledge](#our-pledge)
    - [Our Standards](#our-standards)
    - [Our Responsibilities](#our-responsibilities)
    - [Scope](#scope)
    - [Enforcement](#enforcement)
    - [Attribution](#attribution)

## Getting started

First up, install Forko:

```bash
npm install forko
```

In your Sass (SCSS in this case), import Forko like so:

```scss
@import '../path/to/your/node_modules/forko/forko.scss';
```

This will generate utility classes based on the default configuration. To configure it for yourself, take this default, and create your own. Once it is created **import your config before Forko**, like this:

```bash
@import 'config';
```

## Configuration

This is the default configuration. It is recommended that you use it as your base for your own configuration.

```scss
/// BASE SIZE
/// All calculations are based on this. It’s recommended that
/// you keep it at 1rem because that is the root font size. You
/// can set it to whatever you like and whatever unit you like.
///
$forko-base-size: 1rem;

/// SIZE SCALE
/// This is a Major Third scale that powers all the utilities that
/// it is relevant for (font-size, margin, padding). All items are
/// calcuated off the base size, so change that and cascade across
/// your whole project.
///
$forko-size-scale: (
  '300': $forko-base-size * 0.8,
  '400': $forko-base-size,
  '500': $forko-base-size * 1.25,
  '600': $forko-base-size * 1.6,
  '700': $forko-base-size * 2,
  '900': $forko-base-size * 3
);

/// COLORS
/// Colors are shared between backgrounds and text by default.
/// You can also use them to power borders, fills or shadows, for example.
///
$forko-colors: (
  'dark': #1a1a1a,
  'light': #f3f3f3
);

/// CORE CONFIG
/// This powers everything from utility class generation to breakpoints
/// to enabling/disabling pre-built components/utilities.
///
$forko-config: (
  'bg': (
    'items': $forko-colors,
    'output': 'standard',
    'property': 'background'
  ),
  'color': (
    'items': $forko-colors,
    'output': 'standard',
    'property': 'color'
  ),
  'box': (
    'items': (
      'block': 'block',
      'flex': 'flex',
      'hide': 'none',
      'show': 'inherit'
    ),
    'output': 'responsive',
    'property': 'display'
  ),
  'font': (
    'items': (
      'base': 'Helvetica, Arial, sans-serif'
    ),
    'output': 'standard',
    'property': 'font-family'
  ),
  'gap-top': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'margin-top'
  ),
  'gap-right': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'margin-right'
  ),
  'gap-bottom': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'margin-bottom'
  ),
  'gap-left': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'margin-left'
  ),
  'pad-top': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'padding-top'
  ),
  'pad-right': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'padding-right'
  ),
  'pad-bottom': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'padding-bottom'
  ),
  'pad-left': (
    'items': $forko-size-scale,
    'output': 'standard',
    'property': 'padding-left'
  ),
  'stack': (
    'items': (
      '300': 0,
      '400': 10,
      '500': 20,
      '600': 30,
      '700': 40
    ),
    'output': 'standard',
    'property': 'z-index'
  ),
  'text': (
    'items': $forko-size-scale,
    'output': 'responsive',
    'property': 'font-size'
  ),
  'weight': (
    'items': (
      'light': '300',
      'regular': '400',
      'bold': '700'
    ),
    'output': 'standard',
    'property': 'font-weight'
  ),
  'width': (
    'items': (
      'full': '100%',
      'half': percentage(1/2),
      'quarter': percentage(1/4),
      'third': percentage(1/3)
    ),
    'output': 'responsive',
    'property': 'width'
  ),
  'breakpoints': (
    'sm': '(min-width: 36em)',
    'md': '(min-width: 48em)',
    'lg': '(min-width: 62em)'
  )
);
```

### Base size (optional)

`$forko-base-size`

The base size for the size ratio calculations. It is only required for the default configuration.

### Size scale (optional)

`$forko-size-scale`

This takes the base size and by default, generates a **major third** size scale. This can be set to whatever scale you like.

If this is not set, the `get-size` function will use the default configuration.

### Colors (optional)

`$forko-colors`

A collection of key/value pairs that by default, generate text and background colour utilities.

If this is not set, the `get-color` function will use the default configuration.

### Forko config (required)

`$forko-config`

🚨 Without this set, Forko won’t work. 🚨

It contains all of the utility class definitions and breakpoint definitions that the generator and mixins use.

You can add as many or as little utility class definitions as you like—likewise for breakpoint definitions.

### Breakpoints

The `breakpoints` map in `$forko-config` defines media queries for the utility class generator. By default, the are set as follows:

```scss
'breakpoints': (
	'sm': '(min-width: 36em)',
	'md': '(min-width: 48em)',
	'lg': '(min-width: 62em)'
)
```

You can add as many or as little of these as you like and call them whatever you like. The only requirement is that the value is a valid media query.

## Utility Class Generator

The utility class generator loops through `$forko-config` looking for items that have a valid utility class structure. The following structure is required to generate a utility class:

```scss
'width':('items':('full':'100%','half': '50%'
  ),
  'output': 'standard',
  'property': 'width'
),;
```

The first key is the name of the utility and that contains a Sass map. Inside that map, you need to have the following:

- `items`: a map of key/value pairs which link a utility class to a CSS property’s value. If you want to use CSS Custom Properties, this should be the string key, referencing the `'css-vars'` `$forko-config` group that you want to use
- `output`: this must be `responsive` or `standard`. If you set it to `responsive`, it will generate the same utility class for **every breakpoint that is defined**.
- `property`: the [CSS property](https://css-tricks.com/almanac/properties/) that this utility controls.

### Example outputs

The above structure would output the following utility classes:

```css
.width-full {
  width: 100%;
}

.width-half {
  width: 50%;
}
```

If we set the `output` to be `responsive`, with the default `breakpoints` defined, the output would be as follows:

```css
.width-full {
  width: 100%;
}

.width-half {
  width: 50%;
}

@media (min-width: 36em) {
  .sm\:width-full {
    width: 100%;
  }

  .sm\:width-half {
    width: 50%;
  }
}

@media (min-width: 48em) {
  .md\:width-full {
    width: 100%;
  }

  .md\:width-half {
    width: 50%;
  }
}

@media (min-width: 62em) {
  .lg\:width-full {
    width: 100%;
  }

  .lg\:width-half {
    width: 50%;
  }
}
```

## Generating Utility Classes On Demand

The default behaviour of Forko is to generate utility classes, but in the spirit of being as flexible as possible, you can stop it doing that by setting `$generate-utility-classes` to `false` when you pull Forko into your project, like this:

```scss
$generate-utility-classes: false;
@import 'config';
@import '../path/to/your/node_modules/forko/forko.scss';
```

We might want to generate those utility classes later on in the CSS, though, so we use the `generate-utility-classes()` mixin anywhere **after** Forko has been pulled in.

```scss
$generate-utility-classes: false;
@import 'config';
@import '../path/to/your/node_modules/forko/forko.scss';

// Standard authored CSS
body {
  display: grid;
  place-items: center;
}

// Generate utilities after everything else
@include generate-utility-classes();
```

## Using CSS Custom Properties

**[See a demo repo](https://github.com/andy-piccalilli/forko-custom-props-demo)**

You might want to use CSS Custom Properties instead of static references to tokens. To do so with Forko, you need to make a couple of adjustments to your `$forko-config`.

Firstly, at the top, you need to add a `css-vars` group which has a **key** and a value, which should be a map of tokens.

```scss
$forko-config: (
  'css-vars': (
    'color': $forko-colors,
    'weight': (
      'bold': 700,
      'black': 900
    )
  )
);
```

In this example, we have defined a `'color'` group which uses `$forko-colors`, but also a `'weight'` group where we have defined key value pairs, just like we do in the utility class generator.

This will now generate a collection of CSS Custom properties like this:

```css
:root {
  --color-dark: #1a1a1a;
  --color-light: #f3f3f3;
  --weight-bold: 700;
  --weight-black: 900;
}
```

To use CSS Custom Properties in a utility class, we need to first, switch `'items'` to be a reference to the `'css-vars'` group we want, then set `'css-vars'` to be `true`.

```scss
'bg': (
  'items': 'color',
  'css-vars': true,
  'output': 'standard',
  'property': 'background'
)
```

Now, the background utility classes will look like this:

```css
.bg-dark {
  background: var(--color-dark);
}

.bg-light {
  background: var(--color-light);
}
```

**Note**: You can use a combination of CSS Custom Properties _and_ static references to tokens for different utility classes. Forko is flexible enough to let you do what works for you and your team.

When you enable CSS custom properties, Forko will generate the `:root` blocks for you, but sometimes, you might want those `:root` blocks not to be rendered. This is common if you are generating more than one CSS bundle. **To disable the generation of CSS Custom Property blocks, set `$generate-css-vars = false;`, before you import Forko, just like [generating utility classes on demand](#generating-utility-classes-on-demand)**.

### Using themes

**This feature requires Custom Properties**

A handy part of the Custom Property support with Forko is the ability to generate multiple themes. These themes can power dark mode with `@media (prefers-color-scheme: dark)` or be prefixed with whatever scheme you like.

Let’s say you want a dark mode that [both honours the user’s preference, via a media query, and also, can be toggled](https://piccalil.li/tutorial/create-a-user-controlled-dark-or-light-mode/). The toggle version could use `[data-theme="dark"]` as its prefix. We’ll generate a default light theme too.

First, we set some values.

```scss
$forko-colors: (
  'dark': #1a1a1a,
  'light': #f3f3f3
);

$light-colors: (
  'text': map-get($forko-colors, 'dark'),
  'bg': map-get($forko-colors, 'light')
);

$dark-colors: (
  'text': map-get($forko-colors, 'light'),
  'bg': map-get($forko-colors, 'dark')
);
```

Then, we tweak `$forko-config`.

```scss
$forko-config: (
  'css-vars': (
    'themes': (
      'default': (
        'tokens': (
          'color': $light-colors
        )
      ),
      'dark': (
        'prefers-color-scheme': 'dark',
        'tokens': (
          'color': $dark-colors
        )
      ),
      'dark-toggle': (
        'prefix': '[data-theme="dark"]',
        'tokens': (
          'color': $dark-colors
        )
      )
    )
  ),
  /// the rest of your config
);
```

This then generates the following Custom Properties:

```css
:root {
  --color-text: #1a1a1a;
  --color-bg: #f3f3f3;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-text: #f3f3f3;
    --color-bg: #1a1a1a;
  }
}

[data-theme='dark'] {
  --color-text: #f3f3f3;
  --color-bg: #1a1a1a;
}
```

Now, we can style elements like so and regardless of what theme is selected, we won’t have to change our CSS:

```css
.my-element {
  background: var(--color-bg);
  text: var(--color-text);
}
```

A complete theme map looks like this:

```scss
'dark-toggle': ( // Name required
  'prefix': '[data-theme="dark"]', // Optional. Will be :root if not set
  'prefers-color-scheme': 'dark', // Optional. Will generate @media rule if set
  'tokens': ( // Required. Map of key value pairs
    'color': (
      'primary': #1a1a1a,
      'secondary': #f3f3f
    )
  )
)
```

You can generate as many themes with whatever prefix you can think up!

You could also [generate color utility classes using the generator](#utility-class-generator) that use these custom properties.

## Namespaces
Forko supports 'namespacing' both the generated class and variable names by allowing you to specify a prefix.  This is done using the `namespace` map within `$forko-config`.  The default `namespace` config looks like this:

````scss
$forko-config: (
  'namespace': (
    'prefix': '',             // string
    'classes': true,          // boolean or string
    'css-vars': false         // boolean or string
  )
)
````
### Namespace Settings
#### prefix
Specifying a value for `prefix` will append that value to classes (by default) and css variables (opt-in).  You are responsible for appending any separating character, such as a dash or underscore. For example, if your namespace is `my-app-`, you need to add the trailing `-` character in the `'prefix'` section.

#### classes
Accepts either a boolean value indicating that the `prefix` should be applied to generated utility classes OR a string, which allows you to override the global `prefix`

#### css-vars
Accepts either a boolean value indicating that the `prefix` should be applied to css-vars OR a string, which allows you to override the global `prefix`

### Examples

Minimal Configuration: This configuration would prepend `my-` to the beginning of generated utility classes, but would not modify css variable names:

````scss
$forko-config: (
  'namespace': (
    'prefix': 'my-'
  )
)
````

Everything prefixed:  This configuration applies the prefix to both utility classes and css variables:

````scss
$forko-config: (
  'namespace': (
    'prefix': 'my-',
    'css-vars': true
  )
)
````

Separate prefixes:  This configuration gives you the ability to provide different prefixes for utility classes and css variables:

````scss
$forko-config: (
  'namespace': (
    'classes': 'my-class-',
    'css-vars': 'my-var-'
  )
)
````

## Sass functions

There are a couple of handy functions that give you access to configuration settings.

### Get color

`get-color($key: string)`

Takes the passed `$key` and attempts to retrieve a match from `$forko-colors`.

#### Example

Using the default config:

```scss
$dark = get-color('dark'); // #1a1a1a
```

### Get utility value

`get-utility-value($key: string, $value-key: string)`

Returns back the value for a utility class so you can use it directly.

#### Example

Using the default config:

```scss
font-weight: get-utility-value('weight', 'light'); // 300
```

### Get size

`get-size($ratio-key: string)`

Tries to match the passed `$ratio-key` with the `$forko-size-scale`. Returns null if it can’t find a match.

#### Example

Using the default config:

```scss
$my-size: get-size('500'); // 1.25rem
```

## Sass mixins

### Apply utility

`apply-utility($key: string, $value-key: string)`

Grab the property and value of one of the `$forko-config` utilities that the generator will generate a class for.

#### Example

Using the default config:

```scss
.my-element {
  @include apply-utility('weight', 'bold'); // font-weight: bold;
}
```

### Media query

`media-query($key: string)`

Pass in the key of one of your breakpoints set in `$forko-config['breakpoints']` and this mixin will generate the media query with your configured value.

#### Example

Using the default config:

```scss
.my-element {
  @include media-query('md') {
    background: goldenrod;
  }
}
```

Output:

```css
@media (min-width: 48em) {
  .my-element {
    background: goldenrod;
  }
}
```

## Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change.

Please note we have a code of conduct, please follow it in all your interactions with the project.

## Pull Request Process

1. Ensure any install or build dependencies are removed before the end of the layer when doing a
   build.
2. Ensure your work is thoroughly tested, to the best of your abilities
3. You may merge the Pull Request in once you have the sign-off from a maintainer

## Code of Conduct

### Our Pledge

In the interest of fostering an open and welcoming environment, we as
contributors and maintainers pledge to making participation in our project and
our community a harassment-free experience for everyone, regardless of age, body
size, disability, ethnicity, gender identity and expression, level of experience,
nationality, personal appearance, race, religion, or sexual identity and
orientation.

### Our Standards

Examples of behavior that contributes to creating a positive environment
include:

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

Examples of unacceptable behavior by participants include:

- The use of sexualized language or imagery and unwelcome sexual attention or
  advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information, such as a physical or electronic
  address, without explicit permission
- Other conduct which could reasonably be considered inappropriate in a
  professional setting

### Our Responsibilities

Project maintainers are responsible for clarifying the standards of acceptable
behavior and are expected to take appropriate and fair corrective action in
response to any instances of unacceptable behavior.

Project maintainers have the right and responsibility to remove, edit, or
reject comments, commits, code, wiki edits, issues, and other contributions
that are not aligned to this Code of Conduct, or to ban temporarily or
permanently any contributor for other behaviors that they deem inappropriate,
threatening, offensive, or harmful.

### Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

### Enforcement

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported by contacting the project team at me@andy-bell.design. All
complaints will be reviewed and investigated and will result in a response that
is deemed necessary and appropriate to the circumstances. The project team is
obligated to maintain confidentiality with regard to the reporter of an incident.
Further details of specific enforcement policies may be posted separately.

Project maintainers who do not follow or enforce the Code of Conduct in good
faith may face temporary or permanent repercussions as determined by other
members of the project's leadership.

### Attribution

This Code of Conduct is adapted from the [Contributor Covenant][homepage], version 1.4,
available at [http://contributor-covenant.org/version/1/4][version]

[homepage]: http://contributor-covenant.org
[version]: http://contributor-covenant.org/version/1/4/
