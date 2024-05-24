# HTML

## Boilerplate

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
    ...
</body>
</html>
```

Emmet: `![tab]`



## Elements

### Common Tags

**Comment**  
```html
<!-- comment -->
```

**Anchor**  
```html
<a href="dest"></a>
```

**Image**  
```html
<img src="src" alt="description">
```

**Block**  
```html
<div>...</div>
```

**Inline**  
```html
<span class="class">...</span>
```

**Superscript and Subscript**  
```html
<sup>...</sup>
<sub>...</sub>
```

**Entities**  
```html
<!-- $code; -->
<!-- ampersand -->
&amp;
```

### Semantic Markup
```html
<main>...</main>
<nav>...</nav>

<section>...</section>
<article>...</article>

<aside>...</aside>
<header...></header>
<footer...></footer>

<time>...</time>
<figure>...</figure>
```

### Lists
```html
<ol>
  <li>
    <ul>
      <li>...</li>
      <li>...</li>
      <li>...</li>
    </ul>
  </li>
  <li>...</li>
  <li>...</li>
</ol>
```

### Tables
```html
<table>
  <thead>
    <tr>
      <th rowspan="span">header1</th>
      <th colspan="span">header2</th>
      <th>header3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>data1</td>
      <td>data2</td>
      <td>data3</td>
    </tr>
    <tr>
      <td>data1</td>
      <td>data2</td>
      <td>data3</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td>footer1</td>
      <td>footer2</td>
      <td>footer3</td>
    </tr>
  </tfoot>
</table>
```

### Forms
```html
<form action="dest" method="method">
  <p>
    <label for="id">...</label>
    <input type="type" placeholder="text" id="id" name="param_label">
  </p>
  <!-- or -->
  <p>
    <label>...
      <input type="type" placeholder="text" name="param_label">
    </label>
  </p>

  <button type="submit">...</button>
  <!-- or -->
  <input type="submit" value="value">
</form>
```
Forms support built-in and basic validations
```html
<input type="text" required>
```



## Notes

### Enable auto-format upon save

### Use Emmet shortcuts
Type CSS-like expressions that are automatically expanded to HTML syntax

[Cheat Sheet](https://docs.emmet.io/cheat-sheet/)

### Submit form as object or array
```html
// when received by backend: { entity: {key: value} }
<input name="entity[key]">

// when received by backend: { entity: [...] }
<input name="entity[]">
```