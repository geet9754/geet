document.addEventListener("DOMContentLoaded", () => {
    const searchBar = document.getElementById("search-bar");
    const searchButton = document.getElementById("search-btn");
    const products = document.querySelectorAll(".product-card");

    // Sorting Dropdown
    const sortSelect = document.createElement("select");
    sortSelect.innerHTML = `
        <option value="default">Sort By</option>
        <option value="price-low">Price: Low to High</option>
        <option value="price-high">Price: High to Low</option>
    `;
    document.querySelector("header").appendChild(sortSelect);

    // Search Functionality
    searchButton.addEventListener("click", () => {
        const searchText = searchBar.value.toLowerCase();
        products.forEach(product => {
            const title = product.querySelector("h3").textContent.toLowerCase();
            product.style.display = title.includes(searchText) ? "block" : "none";
        });
    });

    searchBar.addEventListener("keyup", (event) => {
        if (event.key === "Enter") {
            searchButton.click();
        }
    });

    // Sorting Functionality
    sortSelect.addEventListener("change", () => {
        const sortValue = sortSelect.value;
        let sortedProducts = [...products];

        if (sortValue === "price-low") {
            sortedProducts.sort((a, b) => 
                parseFloat(a.querySelector(".price").textContent.slice(1)) - 
                parseFloat(b.querySelector(".price").textContent.slice(1))
            );
        } else if (sortValue === "price-high") {
            sortedProducts.sort((a, b) => 
                parseFloat(b.querySelector(".price").textContent.slice(1)) - 
                parseFloat(a.querySelector(".price").textContent.slice(1))
            );
        }

        const productContainer = document.querySelector(".products");
        productContainer.innerHTML = "";
        sortedProducts.forEach(product => productContainer.appendChild(product));
    });

    // Add to Cart Functionality
    products.forEach(product => {
        const cartButton = document.createElement("button");
        cartButton.textContent = "Add to Cart";
        cartButton.style.background = "#ff6600";
        cartButton.style.color = "#fff";
        cartButton.style.border = "none";
        cartButton.style.padding = "5px 10px";
        cartButton.style.cursor = "pointer";
        product.appendChild(cartButton);

        cartButton.addEventListener("click", () => {
            alert(`${product.querySelector("h3").textContent} added to cart!`);
        });
    });
});
