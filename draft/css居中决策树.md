# CSS居中决策树

## 一、水平

- 是内联的元素还是类内联元素（比如文本或链接）？

```css

在一个块状元素内居中只要给父元素加：

.center-children {
  text-align: center;
}

```

- 是一个块级元素吗？

```css

你可以通过给块级元素 的margin-left 和 margin-right 设置auto (但他的宽度必须设置好 ,否则它将全宽,不需要居中)

.center-me {
  margin: 0 auto;
}

```

- 是多个块级元素吗？

```css

如果你有两个或两个以上的块级元素需要水平居中在一行 ，那可能你就会需要另一种display方式inline-block了，外加flex


除非你的意思是你有多个块级元素堆叠在彼此之上,给每个元素设置margin：0 auto；就行啦

```

## 二、垂直

### 是内联的元素还是类内联元素（比如文本或链接）

- 一行吗？

```css

有时内联/文本元素可以出现垂直居中,只是因为他们用padding填充上方和下方。

.link {
  padding-top: 30px;
  padding-bottom: 30px;
}

如果因为某种原因padding不能用，你想居中一些你知道不会换行的文本你可以设置他的line-height等于height

.center-text-trick {
  height: 100px;
  line-height: 100px;
  white-space: nowrap;
}

```

- 多行吗？

```css

设置上下padding也能给多行文本居中的效果，但如果不行的话。可能这个文本在table表格中。那么vertical-align可能会搞定他

.center-table p {
  display: table-cell;
  margin: 0;
  background: black;
  color: white;
  padding: 20px;
  border: 10px solid white;
  vertical-align: middle;
}

能用flexbox吗？

.flex-center-vertically {
  display: flex;
  justify-content: center;
  flex-direction: column;
  height: 400px;
}

注意，以上两种都要求父盒子有固定的高度
如果没有的话可以使用“Ghost Element”技术

.ghost-center {
  position: relative;
}

.ghost-center::before {
  content: " ";
  display: inline-block;
  height: 100%;
  width: 1%;
  vertical-align: middle;
}

.ghost-center p {
  display: inline-block;
  vertical-align: middle;
}

```

### 是一个块级元素吗？

- 知道高度

```css

.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  height: 100px;
  margin-top: -50px; /* account for padding and border if not using box-sizing: border-box; */
}

```

- 不知道高度

```css
.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

```

- 可以用flexbox

```css

.parent {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

```

## 三、水平和垂直

- 固定宽度和高度的元素吗？

```css

.parent {
  position: relative;
}

.child {
  width: 300px;
  height: 100px;
  padding: 20px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -70px 0 0 -170px;
}

```

- 不固定宽度和高度的元素吗？

```css

.parent {
  position: relative;
}

.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

```

- 可以使用flexbox吗？

```css

.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}

```

- 可以使用grid吗？

```css

body, html {
  height: 100%;
  display: grid;
}

span { /* thing to center */
  margin: auto;
}

```
