i18ng
=====

i18next for Angular.js

[![Gitter](https://badges.gitter.im/mikefrey/i18ng.svg)](https://gitter.im/mikefrey/i18ng)

## Installation

Using Bower:

```
$ bower install i18ng --save
```

### Dependencies

* You'll also need to have i18next installed. (`$ bower install i18next --save`)
* ngSanitize is required for translations containing html elements. (`$ bower install angular-sanitize --save`)


## Setup

```javascript
var app = angular.module('MyApp', ['i18ng', 'ngSanitize'])

app.config(['i18ngProvider', function(i18ngProvider) {

  i18ngProvider.init({
    resGetPath: '/examples/locales/__lng__/__ns__.json'
  })

}])
```

## Usage

### Filter

The filter is simply `t`.

#### simple translations

Translate `hello_world`:
```html
{{ 'hello_world' | t }}
```

Translate `$scope.greeting`:
```
{{ greeting | t }}
```

#### using options

Given the following translation dictionary:
```json
{
  "score": "Score: __score__",
  "lives": "1 life remaining",
  "lives_plural": "__count__ lives remaining"
}
```

Translates `score` with a `score` option:
```html
<!-- With a number literal -->
{{ 'score' | t:{'score':10000} }}
<!-- With a variable on $scope -->
{{ 'score' | t:{'score':playerScore} }}
```

Translates `lives` with a `count` option:
```html
<!-- prints: 3 lives remaining -->
{{ 'lives' | t:{'count':3} }}
<!-- prints: 1 life remaining -->
{{ 'lives' | t:{'count':1} }}
```


#### html values

To render HTML from a translation value, you must use the `ng-bind-html` directive. Angular will escape your html characters if you try `{{ 'translation.with.html' | t }}`.

Given the following translation dictionary:

```json
{
  "strong": "Some <strong>bolded</strong> text."
}
```

Translates `strong` with HTML:
```html
<div ng-bind-html="'strong' | t"></div>
```

Renders: Some <strong>bolded</strong> text.


### Directive

The directive is restricted to attributes. The key attribute is `i18ng`. Other
attributes are use as well, all prefixed by `i18ng-`.

#### simple translations

Translate `hello_world`:
```html
<span i18ng="'hello_world'"></span>
```

Translate `$scope.greeting`:
```
<span i18ng="greeting"></span>
```

#### using options

Given the following translation dictionary:
```json
{
  "score": "Score: __score__",
  "lives": "1 life remaining",
  "lives_plural": "__count__ lives remaining"
}
```

Translates `score` with a `score` option:
```html
<!-- With a number literal -->
<span i18ng="'score'" i18ng-opts="{score:10000}"></span>
<!-- With a variable on $scope -->
<span i18ng="'score'" i18ng-opts="{score:playerScore}"></span>
```

Translates `lives` with a `count` option:
```html
<!-- prints: 3 lives remaining -->
<span i18ng="'lives'" i18ng-opts="{count:3}"></span>
<!-- prints: 1 life remaining -->
<span i18ng="'lives'" i18ng-opts="{count:1}"></span>
```


#### html values

You can choose to support html values coming from your translation keys by including the `i18ng-html` attribute.

Given the following translation dictionary:

```json
{
  "strong": "Some <strong>bolded</strong> text."
}
```

Translates `strong` with HTML:
```html
<span i18ng="'strong'" i18ng-html></span>
```

Renders: Some <strong>bolded</strong> text.

#### nested html `i18ng-nested-html`

For more complex markup (containing application logic), you can use the `i18ng-nested-html` attribute to include tags (or even compiled html) inline with your text.

Given the following translation dictionary:

```json
{
  "text_with_link" : "Here is an <icon>, a <#link-one>link</link-one> and <#link-two>another</link-two>."
}
```

Translates `text_with_link` with nested tags inserted:

```html
<p i18ng-nested-html i18ng="'text_with_link'">
  <a href="http://example.com" class="link" i18ng-tag-name="link-two"></a>
  <a href="http://foo.com" class="link" i18ng-tag-name="link-one"></a>
  <i class="fa fa-trash" i18ng-tag-name="icon"></i>
</p>
```

Renders:

```html
<p>
  Here is an <i class="fa fa-trash"></i>, a <a href="http://foo.com" class="link">link</a>
  and <a href="http://example.com" class="link">another</a>.
</p>
```

**Note:** the order of the inserted tags matches the translation order, not the original source order. Also, any child tags NOT used in the translation will be removed.

#### attributes `i18ng-<attr>`

Translating attributes on an element is supported as well. To add a `title`
attribute, use `i18ng-title`:

```html
<a href="#" i18ng i18ng-title="'click_here'"></a>
```

the key `click_here` will be translated and the result will be placed into a
`title` attribute like so:

```html
<a href="#" title="Click Here!"></a>
```

Attributes even support options:

```html
<span i18ng i18ng-title="'lives'" i18ng-title-opts="{count:3}"></span>
```


### Service

An `i18ng` service is available that can be injectected into your own controllers, directives and services. Use the `t` method to perform the translation.

```js
angular.module('myModule')
  .controller('myCtrlr', function($scope, i18ng) {
    $scope.translatedText = i18ng.t('somekey')
  }])
```
