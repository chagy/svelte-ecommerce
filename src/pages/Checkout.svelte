<script>
  import { onMount } from "svelte";
  import { navigate, link } from "svelte-routing";
  import user from "../stores/user";
  import { cartTotal } from "../stores/cart";

  let name = "";
  let cardElement;
  let cardErrors;
  let card;
  let stripe;
  let elements;

  $: isEmpty = !name;

  onMount(() => {
    if (!$user.jwt) {
      navigate("/");
    }
  });

  function handleSubmit() {}
</script>

{#if $cartTotal > 0}
  <section class="form">
    <h2 class="section-title">checkout</h2>
    <form on:submit|preventDefault={handleSubmit} class="checkout-form">
      <h3>order Total : ${$cartTotal}</h3>
      <div class="form-control">
        <label for="name">your name</label>
        <input type="text" name="name" id="name" bind:value={name} />
      </div>

      <div class="stripe-input">
        <label for="card-element">Credit or Debit Card</label>
        <p class="stripe-info">
          Test using this credit card
          <span>4242 4242 4242 4242</span>
          <br />
          enter any 5 digits for the zip code
          <br />
          enter any 3 digits for the cvc
        </p>
        <div id="card-element" bind:this={cardElement} />
        <div id="card-errors" bind:this={cardErrors} role="alert" />
      </div>

      {#if isEmpty}
        <p class="form-empty">Please fill out name field</p>
      {/if}
      <button
        type="submit"
        class="btn btn-block btn-primary"
        disabled={isEmpty}
        class:disabled={isEmpty}
      >
        submit
      </button>
    </form>
  </section>
{:else}
  <div class="checkout-empty">
    <h2>your cart is empty</h2>
    <a href="/products" use:link class="btn btn-priamry">fill it</a>
  </div>
{/if}
