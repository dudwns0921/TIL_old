```
const plus = document.getElementById("plus");
const minus = document.getElementById("minus");
const number = document.getElementById("number");

let count = 0;

const updateText = () => {
    number.innerText = count;
}

const handlePlus = () => {
    count+=1
    updateText();
}

const handleMinus = () => {
    count-=1
    updateText();
}

plus.addEventListener("click", handlePlus);
minus.addEventListener("click", handleMinus);

```

```
  <body>
    <button id="plus">Add</button>
    <span id="number">0</span>
    <button id="minus">Minus</button>
  </body>
```

reducer는