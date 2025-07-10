
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
