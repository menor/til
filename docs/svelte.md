# SVELTE

## Basic Tips
- Try to keep calculations out of your HTML derive them in your JS using the label statement `$: <variable-name>`. Svelte will recalculate this variable any time the variables it depends on change.
- You can also pass expressions after the `$: ` label statement and they will be rerun every time the depending values change. i.e. `$: console.log(name)`
- Or even change other values when the observed value changes
```js
$: if (name === 'Tiny Rick) {
	age = 13
}
```
	
