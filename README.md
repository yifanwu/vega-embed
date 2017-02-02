# vega-embed

[![npm version](https://img.shields.io/npm/v/vega-embed.svg)](https://www.npmjs.com/package/vega-embed)

The [vega-embed](http://github.com/vega/vega-embed) module provides advanced support for embedding interactive Vega views into web pages. Current version supports only Vega 3 / Vega-Lite 2. The primary features include:

- Load Vega/Vega-Lite specs from source text, parsed JSON, or URLs.
- Add action links such as "View Source" and "Open in Vega Editor".


_As Vega 3's `signal` supports [bind](https://github.com/vega/vega/blob/master/PORTING_GUIDE.md#scales), `parameter` property in the older version is now deprecated._
<!--TODO : Link the older version document -->

## Basic Example
```html
<!DOCTYPE html>
<html>
<head>
  <!-- Please import Vega 3 & Vega-Lite 2 js  -->
  <script src="vega.js" charset="utf-8"></script>
  <script src="vega-lite.js" charset="utf-8"></script>
  <script src="vega-embed.js" charset="utf-8"></script>
</head>
<body>

<div id="vis"></div>

<script type="text/javascript">
  var embed_spec = {
    url: "https://raw.githubusercontent.com/vega/vega/master/spec/airports.vg.json"
  };
  vega.embed('#vis', embed_spec);
</script>
</body>
</html>
```


## API Reference

<a name="embed" href="#embed">#</a>
vega.<b>embed</b>(<i>el</i>, <i>embed_spec</i>[, <i>callback</i>])
[<>](https://github.com/vega/vega-embed/src/embed.js "Source")

The embed function accepts the following arguments:

| Index   | Name         | Description    |
| :------ | :----------- | :------------- |
| 0       | `el`         | A DOM element or CSS selector indicating the element on the page in which to add the embedded view. |
| 1       | `embed_spec` | A JavaScript object containing the embedding specification. |
| 2       | `callback`   | An optional callback function that upon successful parsing will be invoked with the instantiated Vega `View` instance and a copy of the parsed JSON Vega spec. |

```html
<script>
vega.embed('#vis', embed_spec, function(error, result) {
  // Callback receiving the View instance and parsed Vega spec
  // result.view is the View object of Vega 3
  // result.spec is the parsed spec
});
</script>
```

### Vega Embed Specification Reference

```JS
var embed_spec = {
  "source"/"spec"/"url" : ... ,
  "renderer" : ...,
  "actions" : ...,
  "config"  : ...
}
```
Here is the complete vega-embed specification definition, broken down by category.

#### Loading a Vega Specification

| Property | Type             | Description    |
| :------- | :--------------- | :------------- |
| `source` | String | The Vega specification as a JSON text string. The _source_ property takes precedence over both the _spec_ and _url_ properties. |
| `spec`   | Object | The Vega specification as a parsed JSON object. The _spec_ property takes precedence over the _url_ property, but not the _source_. |
| `url`    | String | A URL from which to load the Vega specification. Note that this URL will be subject to standard browser security restrictions. Typically this URL will point to a file on the same host and port number as the web page itself. The _url_ property has lower precedence than either the _source_ or _spec_ properties. |

#### Configuration Propeties

| Property      | Type          | Description    |
| :------------ | :------------ | :------------- |
| `renderer`    | String        | The renderer to use for the view. One of `"canvas"` (default) or `"svg"`. |
| `actions`     | Boolean &#124; Object       | Determines if action links ("Export as PNG/SVG", "View Source", "Open in Vega Editor") are included with the embedded view. If the value is `true` (default), all action links will be shown and none if the value is `false`.  This property can take a key-value mapping object that maps keys (`export`, `source`, `editor`) to boolean values for determining if each action link should be shown.  Unspecified keys will be `true` by default.  For example, if `actions` is `{export: false, source: true}`, the embedded visualization will have two links – "View Source" and "Open in Vega Editor".        |
| `config`      | Object        | An optional object to override the [default configuration options](https://github.com/vega/vega-parser/blob/master/src/config.js) or [specify a theme](https://github.com/vega/vega-parser#configuration-reference). |


<a name="embed.config" href="#embed.config">#</a>
vega.embed.<b>config</b>
[<>](https://github.com/vega/vega-embed/src/embed.js "Source")


The `vega.embed.config` object can cofigure the `vega.embed` function to change the behavior of the action links through the following properties. 

| Property        | Type     | Description    |
| :-------------- | :------- | :------------- |
| `editor_url`    | String   | The URL at which to open embedded Vega specs in a Vega editor. Defaults to `"http://vega.github.io/vega-editor/"`. Internally, vega-embed uses [HTML5 postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) to pass the specification information to the editor. |
| `source_header` | String   | HTML to inject into the `<head>` tag of the page generated by the "View Source" action link. For example, this can be used to add code for [syntax highlighting](https://highlightjs.org/). |
| `source_footer` | String   | HTML to inject into the end of the page generated by the "View Source" action link. The text will be added immediately before the closing `</body>` tag. |



## Build Process

To build `vega-embed.js` and view the test examples, you must have [npm](https://www.npmjs.com/) installed.

1. Run `npm install` in the vega-embed folder to install dependencies.
2. Run `npm run build`. This will invoke [browserify](http://browserify.org/) to bundle the source files into vega-embed.js, and then [uglify-js](http://lisperator.net/uglifyjs/) to create the minified vega-embed.min.js.
3. Run a local webserver (e.g., `python -m SimpleHTTPServer 8000`) in the vega-embed folder and then point your web browser at the test page (e.g., `http://localhost:8000/test.html`).
