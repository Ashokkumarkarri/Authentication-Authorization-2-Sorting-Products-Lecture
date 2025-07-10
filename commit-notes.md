

# ✅ Lecture: Adding Sort by Price & React Icons

# Commit 1: importing and using the component 

In this lecture, we will learn:

- How to add **"Sort by Price"** to the Products section.
- How to add **React Icons** using a 3rd-party package.

---

## 🧩 Step 1: Implementing the ProductsHeader Component

We already have a `ProductsHeader` component provided. We will now implement it.

This component will accept 3 props:

### Props accepted by `ProductsHeader`:

1. `sortbyOptions`  
   → An array of sort options (Ex: Price high to low, low to high)

2. `activeOptionId`  
   → The current selected sort option

3. `updateActiveOptionId`  
   → A callback function to update the selected sort option when the user changes it.

---

### ✅ ProductsHeader Code

```js
import './index.css'

const ProductsHeader = props => {
  const {sortbyOptions, activeOptionId, updateActiveOptionId} = props

  const onChangeSortby = event => {
    updateActiveOptionId(event.target.value)
  }

  return (
    <div className="products-header">
      <h1 className="products-list-heading">All Products</h1>
      <div className="sort-by-container">
        <h1 className="sort-by">Sort by</h1>
        <select
          className="sort-by-select"
          value={activeOptionId}
          onChange={onChangeSortby}
        >
          {sortbyOptions.map(eachOption => (
            <option
              key={eachOption.optionId}
              value={eachOption.optionId}
              className="select-option"
            >
              {eachOption.displayText}
            </option>
          ))}
        </select>
      </div>
    </div>
  )
}

export default ProductsHeader
```

---

## 🛠️ Step 2: Using the ProductsHeader Component

We already have a heading in the `AllProductsSection` (ex: "All Products").

Now we want to replace that simple heading with our reusable `ProductsHeader` component.

Here’s how we update the code:

### Updated `renderProductsList()` function:

```js
// In AllProductsSection component
renderProductsList = () => {
  const {productsList} = this.state

  return (
    <>
      <ProductsHeader />
      <ul className="products-list">
        {productsList.map(product => (
          <ProductCard productData={product} key={product.id} />
        ))}
      </ul>
    </>
  )
}
```

---

In the next commit, we will handle the logic to actually sort the products based on the selected option from the header.
---
# ✅ Commit 2: Passing Props to Components

In this commit, we are focusing on **passing props to a child component** (`ProductsHeader`).

We already have an array of options for sorting products. Now, we will pass this array as props to the `ProductsHeader` component.

---

## 📦 Sort Options Array

```js
const sortbyOptions = [
  {
    optionId: 'PRICE_HIGH',
    displayText: 'Price (High-Low)',
  },
  {
    optionId: 'PRICE_LOW',
    displayText: 'Price (Low-High)',
  },
]
```

## 📌 Why use state for activeOptionId?
Since activeOptionId will change over time when the user selects a different sort option, we should keep this value in the component's state.

We know that anything that changes over time should be stored in the state.

```js
state = {
  activeOptionId: sortbyOptions[0].optionId,
  productsList: [],
  isLoading: false,
}
```

### Explanation:
* activeOptionId: The currently selected sort option (initially the first option).

* productsList: The list of products we will render.

* isLoading: Used to show loading state while fetching products.

### 📤 Passing Props to ProductsHeader

Now we pass `sortbyOptions` and `activeOptionId` as props to the `ProductsHeader` component like this:

```js

const {productsList, activeOptionId} = this.state

return (
  <>
    <ProductsHeader
      sortbyOptions={sortbyOptions}
      activeOptionId={activeOptionId}
    />
    <ul className="products-list">
      {productsList.map(product => (
        <ProductCard productData={product} key={product.id} />
      ))}
    </ul>
  </>
)

```
---
# ✅ Commit 3: Sorting Functionality

To make sorting work in our e-commerce app, we need to complete the following two steps:

### ✅ Step 1: Update `activeOptionId` when the user selects a sort option

### ⏳ Step 2: Fetch the sorted product list using the Get Products API

---

### 🔧 Step 1 – Handling the selected sort option

We'll pass a callback function `updateActiveOptionId` as a prop to the `ProductsHeader` component.  
This function will be triggered when the user changes the selected option in the dropdown.

```js
//this will update the state with the curren selected option
updateActiveOptionId = activeOptionId => {
  this.setState({activeOptionId: activeOptionId})
}
```
Now, we pass all required props including the callback to ProductsHeader:

```js
<ProductsHeader
  sortbyOptions={sortbyOptions}
  activeOptionId={activeOptionId}
  updateActiveOptionId={this.updateActiveOptionId}
/>
```

⏭️ Next Commit:

In the next commit, we will handle Step 2, where we'll call the API and fetch the sorted product list based on the selected sort option.

---

---

# ✅ Commit 4: API Call for Sorting the Products

---

## 🧐 Why Do We Need an API Call?

Generally, **backend** is a more efficient and secure place to handle operations like sorting, filtering, and searching on data compared to doing it in the frontend.

---

## 🧩 How Do We Get Sorted Products from the Backend?

We use **query parameters** in the API URL.

For example:

https://apis.ccbp.in/products?sort_by=PRICE_HIGH 

Here, `sort_by=PRICE_HIGH` is the **query parameter**.

---

## 🧠 Getting the Selected Value from State

We already have `activeOptionId` in our state which tracks the selected sort option:

```js
const {activeOptionId} = this.state
```

Using it, we modify the API URL dynamically:
```js
const apiUrl = `https://apis.ccbp.in/products?sort_by=${activeOptionId}`

```

## 🔁 When Should We Call the API?
Currently, we are calling the `getProducts() `function inside `componentDidMount()`:

```js
componentDidMount() {
  this.getProducts()
}
```
But `componentDidMount()` is called only once, when the component is mounted.

## ❗ Our Requirement:
We want to re-fetch the products every time the user selects a new sort option (i.e., when the state changes).

So, we should call the API whenever `activeOptionId is updated.

## ✅ Solution: Call API Inside setState Callback
If we write like this:
```js
updateActiveOptionId = activeOptionId => {
  this.setState({
    activeOptionId
  })
  this.getProducts()
}
```
It won't work as expected because setState is asynchronous, meaning the state might not be updated before the API call is made.

### 🛠️ Proper Way: Use setState Callback
`setState` accepts a callback function which runs after the state has been updated.

```js
this.setState({property1: value1, ...}, callbackFunction)
```
So we update our function like this:

```js
updateActiveOptionId = activeOptionId => {
  this.setState({activeOptionId}, this.getProducts)
}
```
Now, getProducts will be called only after activeOptionId is updated!


---

# ✅ Commit 5: Third-Party Package – React Icons

---

## 📦 npm Installation

Install the package using the following command:

```bash
npm install react-icons
```


Official Docs: https://react-icons.github.io/react-icons/

## 🎨 What is React Icons?
react-icons is a third-party package that provides a bundle of popular icon libraries, such as:

 * Bootstrap Icons

  * Font Awesome

  * Material Design Icons

  * Feather Icons

  * and many more...

## 🧰 2.1 Installing React Icons
To use icons from any of the above libraries in your React project, install react-icons using the command:

```js
npm install react-icons

```

## 📥 2.3 Importing React Icons
Each icon library (category) has its own import path.

The first letters of the icon indicate the category it belongs to.

Example:
If you're using a Font Awesome icon like FaSearch, you must import it like this:

```js
import { FaSearch } from 'react-icons/fa'


```

If you're using a Material Design icon like MdHome, use:

```js
import { MdHome } from 'react-icons/md'


```

To find the icon you want:

  * Visit the official React Icons website.

  * Search for the icon.

  * Copy the import statement from the site.


  -----

  