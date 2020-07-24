<script>
  import { afterUpdate, createEventDispatcher } from "svelte";
  export {
    label,
    value,
    className as class,
    errorMessage,
    placeholder,
    disabled,
    required
  };

  const dispatch = createEventDispatcher();

  let element;
  let value;
  let allowedChars;
  let specialChar;
  let max;
  let min;
  let errorMessage;
  let step = "0";

  let label = "";
  let className = "";
  let placeholder = "";
  let disabled = false;
  let required = false;

  afterUpdate(() => {
    value = value === undefined ? "" : value;
  });

  const handleInput = event => {
    handleValidation(event);

    if (!disabled) {
      dispatch("input", {
        input: value
      });
    }
  };

  const handleValidation = event => {
    if (event.key === "ArrowUp" || event.key === "ArrowDown") {
      event.preventDefault();
    }

    if (!event.target.validity.valid) {
      errorMessage = event.target.validationMessage;
    } else {
      errorMessage = "";
    }
  };
</script>

<style>
  .wrapper {
    flex-direction: column;
  }
  .wrapper:not(:first-child) {
    margin: 1.5rem 0;
  }
  .label-box {
    display: block;
  }
  .label-box label {
    font-style: normal;
    font-weight: 600;
    font-size: 14px;
    line-height: 16px;
  }
  .label-box label.error-message {
    color: #ff3b30;
    text-transform: none;
    letter-spacing: 0.4px;
  }
  .wrapper input {
    padding: 1rem 0;
    background: transparent;
    border: none;
    border-bottom: 1px solid #dce1e1;
    transition: border 0.5s ease-in-out;
  }
  .wrapper input:focus {
    outline: none;
    border-bottom: 1px solid #4842ee;
  }
  .wrapper input:invalid {
    border: 1px solid #ff3b30;
  }
  .wrapper input:disabled {
    background: transparent;
    cursor: default;
  }

  input[type="number"]::-webkit-inner-spin-button,
  input[type="number"]::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
</style>

<div class={'flex wrapper' + className}>
  <div class="label-box">
    {#if label !== ''}
      <label>{label}</label>
    {/if}
    {#if errorMessage}
      <label class="error-message">{errorMessage}</label>
    {/if}
  </div>
  <input
    type="number"
    class:disabled
    bind:value
    bind:this={element}
    on:input={event => handleInput(event)}
    {max}
    {min}
    {step}
    {placeholder}
    {disabled}
    {required} />
</div>
