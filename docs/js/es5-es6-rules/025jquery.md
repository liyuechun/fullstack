# jQuery

## jQuery对象变量的前缀为`$`

```javascript
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```

## 缓存jQuery对象


```javascript
// bad
function setSidebar() {
    $('.sidebar').hide();

    // ...stuff...

    $('.sidebar').css({'background-color': 'pink'});
}

// good
function setSidebar() {
    const $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...stuff...

    $sidebar.css({'background-color': 'pink'});
}
```

## 使用`find`进行查找

```javascript
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar')
    .find('ul')
    .hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar
    .find('ul')
    .hide();
```

