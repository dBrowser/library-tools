# @dbrowser/library-tools

Utility library to manage dBrowser's library of dvaults.

## Library folder and library.json

dBrowser manages a `/library` folder in the user's private ddrive that contains mounts to all saved dvaults. The archives are sorted into folders according to their "Primary Type" (see `getPrimaryType()`). An example folder hierarchy:

```
/library/modules
/library/people
/library/templates
/library/themes
/library/websites
/library/library.json
```

The `library.json` file is the primary record of the "saved dvaults." It lists saved dvaults along with additional configuration. Here is an example:

```json
{
  "type": "dbrowser.com/library",
  "dvaults": [
    {
      "key": "9892414902579ba93ae62e63d5041b69b137f2aa9e86a4e2bc1bcef70ab6d569",
      "isHosting": true,
      "visibility": "public",
      "savedAt": "2019-08-26T15:02:03.623Z"
    },
    {
      "key": "206bf21c61a8c65e3fb1463d6d9b8231dc2726e795009139747a9152f47bb8f7",
      "isHosting": true,
      "visibility": "unlisted",
      "savedAt": "2019-08-26T15:02:03.623Z"
    }
  ]
}
```

The json-schema for library.json [can be found here](./library.json).

## "Categories"

DWebX archives provide a `type` field in their manifests. The "category" is a shortname mapping to those full types.

If multiple types are set on a dwebx, the first known type is used to determine the category.

The "website" category is catchall for dvaults with an unknown type.

## API

### `typeToCategory(type, usePlural=false)`

Provides the category for a given manifest's full type value.

```js
libTools.typeToCategory('dstatus/person', false)
// => 'person'
libTools.typeToCategory('dstatus/person', true)
// => 'people'
```

### `categoryToType(category)`

Provides the full type for a given category. Can use the plural or singular.

```js
libTools.categoryToType('person')
// => 'dstatus/person'
libTools.categoryToType('people')
// => 'dstatus/person'
```

### `getCategoriesArray(usePlural=false)`

Gives an array of the categories.

```js
libTools.getCategoriesArray(false)
// => ['module', 'person', 'template', 'website']
libTools.getCategoriesArray(true)
// => ['modules', 'people', 'templates', 'websites']
```

### `getCategoriesMap(usePlural=false)`

Gives a map of the categories to their fully-qualified types.

```js
libTools.getCategoriesMap(false)
/* => {
  module: 'dstatus/module',
  person: 'dstatus/person',
  template: 'dstatus/template',
  website: undefined
} */
libTools.getCategoriesMap(true)
/* => {
  modules: 'dstatus/module',
  people: 'dstatus/person',
  templates: 'dstatus/template',
  websites: undefined
} */
```

### `categoryToPlural(category)`

Convert a category to its plural.

```js
libTools.categoryToPlural('person')
// => 'people'
```

### `categoryToSingular(category)`

Convert a category to its singular.

```js
libTools.categoryToSingular('people')
// => 'person'
```

### `getFAIcon(category)`

Gets the Font Awesome icon for a given category.

```js
libTools.getFAIcon('module')
// => 'fas fa-cube'
libTools.getFAIcon('person')
// => 'far fa-user'
libTools.getFAIcon('template')
// => 'fas fa-pencil-ruler'
libTools.getFAIcon('theme')
// => 'fas fa-drafting-compass'
```