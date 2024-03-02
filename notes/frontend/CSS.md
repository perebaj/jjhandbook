# CSS Explained

## Class Selectors

```css
.classexample {
  color: red;
}
```

```html
<div class="classexample">Hello World</div> // Here we are using the class selector and passing the class name that was defined in the css file
```

## Flex Box

**flex-direction**: Basically, it defines the direction of a hidden arrow, that could point vertically or horizontally.

```css
.flexbox {
  display: flex;
  flex-direction: row; // Default
  flex-direction: row-reverse;
  flex-direction: column;
  flex-direction: column-reverse;
}
```

**justify-content**: According to the flex-direction, it will define where the items will be positioned. Like:

```css
.flexbox {
  display: flex;
  flex-direction: row;
  justify-content: flex-start; // Default
  justify-content: flex-end;
  justify-content: center;
}
```


[Learn Flexbox CSS in 8 minutes](https://www.youtube.com/watch?v=phWxA89Dy94)
[The perfect Spacing Framework in UI](https://www.youtube.com/watch?v=Al1xsdol4Pk)

The to use the space between elements inside a frame lesser than the padding, and always use a multiple of 4 to define these spaces. (Four point grid system)
