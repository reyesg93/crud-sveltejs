<script>
	import { v4 } from "uuid";
	import Noty from 'noty';

	import 'noty/lib/noty.css';
	import 'noty/lib/themes/nest.css';

  let products = [
    
  ];

  let product = {
    id: "",
    name: "",
    description: "",
    imageURL: "",
    category: ""
  };

  let editStatus = false;

  const cleanProduct = () => {
    product = {
      id: "",
      name: "",
      description: "",
      category: "",
      imageURL: ""
    };
  };

  const addProduct = () => {
    const newProduct = { ...product, id: v4() };

    products = products.concat(newProduct);
    cleanProduct();
  };

  const updateProduct = () => {
    let updatedProduct = {
      id: product.id,
      name: product.name,
      description: product.description,
      category: product.category,
      imageURL: product.imageURL
    };

    const productIndex = products.findIndex(p => p.id === product.id);
    products[productIndex] = updatedProduct;
    cleanProduct();
		let editStatus = false;
		
		new Noty({
			theme: 'nest',
			type: 'success',
			timeout: 3000,
			text: 'Product Update Successfully'
		}).show();
  };

  const onSubmitHandler = e => {
    e.preventDefault();
    if (!editStatus) {
      addProduct();
    } else {
      updateProduct();
    }
  };

  const deleteProduct = id => {
    products = products.filter(product => product.id !== id);
  };

  const editProduct = productEdited => {
    product = productEdited;
    editStatus = true;
  };
</script>

<style>

</style>

<main>
  <div class="container p-4">
    <div class="row">
      <div class="col-md-6">
        {#each products as product}
          <div class="card">
            <div class="row">
              <div class="col-md-4">
                {#if !product.imageURL}
                  <img
                    src="images/no_products.jpg"
                    alt=""
                    class="img-fluid p-2" />
                {:else}
                  <img src={product.imageURL} alt="" class="img-fluid p-2" />
                {/if}
              </div>
              <div class="col-md-8">
                <div class="card-body">
                  <h5>
                    <strong>{product.name}</strong>
                    <span>
                      <small>{product.category}</small>
                    </span>
                  </h5>
                  <p class="card-text">{product.description}</p>
                  <button
                    class="btn btn-danger"
                    on:click={deleteProduct(product.id)}>
                    Delete
                  </button>
                  <button
                    class="btn btn-secondary"
                    on:click={editProduct(product)}>
                    Edit
                  </button>
                </div>
              </div>
            </div>
          </div>
        {/each}
      </div>

      <div class="col-md-6">
        <div class="card">
          <div class="card-body">
            <form on:submit={onSubmitHandler}>
              <div class="form-group">
                <input
                  class="form-control"
                  bind:value={product.name}
                  type="text"
                  id="name-product"
                  placeholder="Product Name" />
              </div>
              <div class="form-group">
                <textarea
                  class="form-control"
                  bind:value={product.description}
                  id="product-description"
                  rows="3" />
              </div>
              <div class="form-group">
                <textarea
                  class="form-control"
                  bind:value={product.imageURL}
                  id="product-image-url"
                  rows="3" />
              </div>
              <div class="form-group">
                <select
                  bind:value={product.category}
                  id="category"
                  class="form-control">
                  <option value="laptops">Laptops</option>
                  <option value="servers">Servers</option>
                  <option value="peripherial">Peripherical</option>
                </select>
              </div>

              <button class="btn btn-secondary">
                {#if !editStatus}Save Product{:else}Update Product{/if}
              </button>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</main>
