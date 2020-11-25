# JS Codes

## Add items to HTML

```jsx
// getting the new added item and add it to UI list
const renderRecipe = recipe => {
    const markup = `
        <li>
            <a class="results__link" href="#${recipe.recipe_id}">
                <figure class="results__fig">
                    <img src="${recipe.image_url}" alt="${recipe.title}">
                </figure>
                <div class="results__data">
                    <h4 class="results__name">${recipe.title}</h4>
                    <p class="results__author">${recipe.publisher}</p>
                </div>
            </a>
        </li>
    `;
    
    // insert html code into the DOM
    elements.searchResultList.insertAdjacentHTML('beforeend', markup);
};

---------------------------------------------------

addListItem: function(obj, type) {
    var html, newHtml, element;

    // create html string with placeholder text
    if (type === 'inc') {
        element = DOMStrings.incomeContainer;
        html = '<div class="item clearfix" id="inc-%id%"><div class="item__description">%description%</div><div class="right clearfix"><div class="item__value">%value%</div><div class="item__delete"><button class="item__delete--btn"><i class="ion-ios-close-outline"></i></button></div></div></div>';
    } else if (type === 'exp') {
        element = DOMStrings.expenseContainer;
        html = '<div class="item clearfix" id="exp-%id%"><div class="item__description">%description%</div><div class="right clearfix"><div class="item__value">%value%</div><div class="item__percentage">21%</div><div class="item__delete"><button class="item__delete--btn"><i class="ion-ios-close-outline"></i></button></div></div></div>';
    }

    // replace the placeholder text with some actual data
    newHtml = html.replace('%id%', obj.id);
    newHtml = newHtml.replace('%description%', obj.desc);
    newHtml = newHtml.replace('%value%', formatNumber(obj.value, type));

    // insert html code into the DOM
    document.querySelector(element).insertAdjacentHTML('beforeend', newHtml);
}
```

## Remove Item from HTML

- `parentElement` is new to Firefox 9 and to DOM4, but it has been present in all other major browsers for ages.
- In most cases, it is the same as `parentNode`. The only difference comes when a node's `parentNode` is not an element. If so, `parentElement` is `null`.

```jsx
// removing element from DOM
deleteListItem: function(selectorID) {
    var element = document.getElementById(selectorID);
    element**.parentNode.removeChild(element);**
}

// Method 2:
loader**.parentElement.removeChild(loader);**
```

## Loader

```jsx
// passing a parent element, so it specify where to insert the child element.
export const renderLoader = parent => {
    const loader = `
        <div class="${elements.loader}">
            <svg>
                <use href="img/icons.svg#icon-cw"></use>
            </svg>
        </div>
    `;
    parent.insertAdjacentHTML('afterbegin', loader);
};

export const clearLoader = () => {
    const loader = document.querySelector(`.${elements.loader}`);
    if (loader) loader.parentElement.removeChild(loader);
};

```

```css
.loader {
  margin: 5rem auto;
  text-align: center; }
  .loader svg {
    height: 5.5rem;
    width: 5.5rem;
    fill: #F59A83;
    transform-origin: 44% 50%;
    animation: rotate 1.5s infinite linear; }
    
/* rotating 360 degree */
@keyframes rotate {
  0% {
    transform: rotate(0); }
  100% {
    transform: rotate(360deg); } }
```

## Limit the length of a String

```jsx
const limitRecipeTitle = (title, limit = 17) => {
    const newTitle = [];
    if (title.length > limit) {
        title.split(' ').reduce((acc, cur) => {
            if ((acc + cur.length) <= limit) {
                newTitle.push(cur);
            }
            return acc + cur.length;
        }, 0);

        // return the new title
        return `${newTitle.join(' ')} ...`;
    }
    return title;
};
```

## Pagination

- Define `data-*` attribute in the HTML element, and we can trace this data by using `element.dataset.*`

```jsx
/**
 * Create a HTML button based on it's type and assign the page num to it.
 * @param {number} page - the current page
 * @param {string} type - the button's type: prev or next
 */
const createButton = (page, type) => `
    <button class="btn-inline results__btn--${type}" data-goto=${type === 'prev' ? page - 1 : page + 1}>
				<span>Page ${type === 'prev' ? page - 1 : page + 1}</span>
        <svg class="search__icon">
            <use href="img/icons.svg#icon-triangle-${type === 'prev' ? 'left' : 'right'}"></use>
        </svg>
    </button>
`;

/**
 * Assign the pagination button(s) to the HTML page.
 * If the page num is 1, then it will only shows the 'next' button.
 * If the page is the last page, it will only shows the 'prev' button.
 * If the page is within the total page number, then shows two buttons.
 * @param {number} page - the current page number
 * @param {number} numResults - the number of results needed to be paginated
 * @param {number} resPerPage - the number of results for each page
 */
const renderButtons = (page, numResults, resPerPage) => {
    const pages = Math.ceil(numResults / resPerPage);
    let button;

    if (page === 1 && pages > 1) {
        // only show the 'next' button
        button = createButton(page, 'next');
    } else if (page < pages) {
        // show 'next' and 'prev' buttons
        button = `
            ${createButton(page, 'prev')}
            ${createButton(page, 'next')}
        `;
    } else if (page === pages && pages > 1) {
        // only show the 'prev' button
        button = createButton(page, 'prev');
    }

    elements.searchResPages.insertAdjacentHTML('afterbegin', button);
};

// loading 10 resources per page by default.
export const renderResults = (recipes, page = 1, resPerPage = 10) => {

    const start = (page - 1) * resPerPage;
    const end = page * resPerPage;

    // calling renderRecipe function to create HTML element for each recipe
    recipes.slice(start, end).forEach(renderRecipe);

    // generate the pagination button(s) based on the page num
    renderButtons(page, recipes.length, resPerPage);
};
```

## Convert Number To Fraction

```jsx
/**
 * Convert a number which may contain decimal part into fraction.
 * @param {number} num 
 */
const formatNumberToFraction = num => {
    if (num) {
        // spliting num to integer and decimal parts.
        // 3.5 => int = 3, dec = 5
        const [int, dec] = num.toString().split('.').map(el => parseInt(el, 10));

        // if there is no decimal part, simply return the num.
        if (!dec) return num;

        // if the integer part is 0, it means 0 < num < 1
        // num = 0 cannot be passed in this if-statement
        if (int === 0) {
            const fr = new Fraction(num);
            return `${fr.numerator}/${fr.denominator}`; // i.e. 2/3
        } else {
            // getting the decimal part
            const fr = new Fraction(num - int);
            return `${int} ${fr.numerator}'/'${fr.denominator}`; // i.e. 1 2/3
        }

    }
    return '?';
};
```

## Generate Unique ID

- using a package
    - `npm install uniqid --save`
    - import it in js file
        - `import uniqid from 'uniqid';`
    - Use it
        - `id: uniqid()`

## 轮子

创建 一个空的array，留10个坑

![JS%20Codes%2070a8bfd61a6747bd87204176e18e803d/Untitled.png](JS%20Codes%2070a8bfd61a6747bd87204176e18e803d/Untitled.png)

```jsx
new Array(10); // 没有下表

Array.apply(Array, {length: 10})
```

The main difference is that `Array(3)` creates an array with three indices that are empty. In fact, they don't really exist, the array just have a length of `3`.

Passing in such an array with empty indices to the Array constructor using `apply` is the same as doing `Array(undefined, undefined, undefined);`, which creates an array with three `undefined` indices, and `undefined` is in fact a value, so it's not empty like in the first example.

Array methods like `map()` can only iterate over actual values, not empty indices.

### 做页数按钮

![JS%20Codes%2070a8bfd61a6747bd87204176e18e803d/Untitled%201.png](JS%20Codes%2070a8bfd61a6747bd87204176e18e803d/Untitled%201.png)

can reduce(), concat()