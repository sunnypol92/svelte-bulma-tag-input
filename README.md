
<h1>Svelte tags input using Bulma</h1>
<div>A rich component for input tags with help of Bulma css</div>
<br />

## Install

```bash
npm install svelte-bulma-tags-input --save
```

```javascript
import Tags from "svelte-bulma-tags-input";

<Tags bind:tags maxTags={5} />
```

```javascript
@import "<PATH>/node_modules/svelte-bulma-tag-input/src/styles";
```
## Options

| Option | Type | Default | Description |
| --- | --- | --- | --- |
| addKeys | `Array` | <kbd>ENTER</kbd> 13 | Set which keys add new values |
| removeKeys | `Array` | <kbd>BACKSPACE</kbd> 8 | Set which keys remove new values |
| maxTags | `Number` | 5 | Set maximum number of tags |
| disabled | `Boolean` | `false` | Disable input |
| hasError | `Boolean` | `false` | Set input error state |
| allowDuplicates | `Boolean` | `false` | Set the entered tags to be duplicatable |
| tagColor | `String` | `primary` | Set the tag color |
| tagSize | `String` | `normal` | Set the tag size |
| tagSize | `String` | `normal` | Set the tag size |
| placeholder | `String` | `null` | Set a placeholder |
| on:change | `Function` | `undefined` | To get the event fire from the tag change |

##### [A complete list of key codes](https://keycode.info/)

## License

This project is open source and available under the [MIT License](LICENSE).

## Author

[Suneesh S K](https://github.com/sunnypol92/svelte-bulma-tag-input)

##### 2021
