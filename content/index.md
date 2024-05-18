<h1>ARE YOU HERE?</h1>

**Some bold text**

[Go to maximum](/maximum.html)

*Hello there, Ruby!*
<button id="click-me">Click me!</button>
<script>
    const clickMe = document.querySelector('#click-me')
    clickMe.addEventListener('click', function () {
        clickMe.textContent = clickMe.textContent === 'Click me!' || clickMe.textContent === 'Click me again!' ? 'Thanks!' : 'Click me again!'
    })
</script>