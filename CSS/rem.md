### rem
rem的值相当于根元素font-size值的倍数
1rem = 根元素font-size
DOMContentLoaded事件动态设置根元素font-size:html.style.fontSize = window.innerWidth / 10 + 'px'

```javascript
document.addEventListener('DOMContentLoaded',()=>{
	const html = document.querySelector('html')
	html.style.fontSize = window.innerWidth / 10 + 'px'
})
```