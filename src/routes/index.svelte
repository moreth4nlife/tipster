<script>
  import { afterUpdate } from "svelte";
  import { fade } from "svelte/transition";
  import { NumberInput, RangeSlider } from "../components/index";

  let percentage = 5;
  let total = 0;
  let people = 1;
  let tip = 0;

  const restaurants = [
    "Island Grill",
    "Flavoroso",
    "Bangalore Spices",
    "Salty Squid",
    "Sweet Escape",
    "Brazilian Steakhouse"
  ];

  const decreasePercentage = event => {
    if (percentage === 5) {
      event.preventDefault();
      return;
    }
    percentage -= 5;
  };

  const increasePercentage = event => {
    if (percentage === 25) {
      event.preventDefault();
      return;
    }
    percentage += 5;
  };

  const getDescription = percentage => {
    const descriptions = {
      5: "I liked it but it could have been better.",
      10: "I enjoyed it very much.",
      15: "I'm definitely coming back to this place.",
      20: "Amazing! I loved it here",
      25: "Unbelieveble! What an experience."
    };
    return descriptions[percentage];
  };

  const countTipAmount = () => {
    tip = (total * Number((1 / 100) * percentage).toFixed(2)) / people;
    tip = Math.round(tip * 100) / 100;
    tip = tip.toFixed(2);
  };

  const checkForEmptyString = () => {
    if (total === "") {
      total = 0;
    }
    if (people === "") {
      people = 0;
    }
  };

  const getRestaurant = () => {
    return restaurants[Math.floor(Math.random() * restaurants.length)];
  };

  afterUpdate(() => {
    checkForEmptyString();
    countTipAmount();
  });
</script>

<style>
  .content-wrapper {
    width: 100vw;
    height: 100vh;
  }
  .content-block {
    flex-direction: column;
    border-radius: 20px;
    width: 375px;
    height: 700px;
    border: 4px solid black;
    background: white;
    box-shadow: 0 3px 6px 0 rgba(0, 0, 0, 0.08), 0 3px 6px 0 rgba(0, 0, 0, 0.08);
  }
  .header {
    padding: 1.5rem;
    border-radius: 15px 15px 0 0;
    background: #f9f8f9;
  }
  .header {
    height: 50px;
  }
  .heading {
    font-size: 24px;
    font-weight: bold;
  }
  .body {
    flex-direction: column;
    height: auto;
    background: #f9f8f9;
  }
  .input-block {
    padding: 1.5rem;
  }
  .result-block {
    padding: 1.5rem 2rem;
    background: white;
    height: 100px;
    border-radius: 30px 0 15px 15px;
  }
  .heading-result {
    font-size: 40px;
    font-weight: bold;
    color: #393839;
  }
  .currency {
    color: #d6d6d7;
  }
  .footer {
    padding: 1.5rem;
    height: 200px;
    border-radius: 30px 0 15px 15px;
    background: #4842ee;
  }
  .footer h3 {
    font-weight: bold;
    color: white;
  }
  .percentage-control {
    margin: 1rem 0;
    user-select: none;
  }
  .percentage-control .button {
    width: 20px;
    height: 20px;
    border: 2px solid white;
    border-radius: 50px;
    color: white;
    transition: opacity 0.25s ease-in-out;
  }
  .percentage-control .button:hover {
    cursor: pointer;
  }
  .percentage-control .button.disabled {
    opacity: 0.5;
  }
  .percentage-control .button span {
    font-size: 16px;
    font-weight: bold;
    user-select: none;
  }
  .description {
    color: white;
  }
  @media screen and (max-width: 667px) {
    .content-block {
      border: none;
    }
    .footer {
      border-radius: 30px 0 0 0;
    }
  }
</style>

<svelte:head>
  <title>Tipster - Calculate Your Tips</title>
</svelte:head>

<section class="flex center content-wrapper">
  <div class="flex space-between content-block">
    <div class="header">
      <span class="heading">Tip Calculator</span>
    </div>
    <div class="flex space-between body">
      <div class="input-block">
        <NumberInput
          label="How much was your bill?"
          on:input={event => {
            total = event.detail.input;
          }} />
        <NumberInput
          label="How many people will share the bill?"
          on:input={event => {
            people = event.detail.input;
          }} />
      </div>
      <div class="flex v-center result-block">
        <span class="heading-result">
          <span class="currency">$</span>
          {tip === isNaN(tip) ? '' : tip}
        </span>
      </div>
    </div>
    <div class="footer">
      <RangeSlider
        value={percentage}
        on:input={event => {
          percentage = event.detail;
        }} />
      <div class="flex space-between percentage-control">
        <div
          class="flex center button button-minus"
          class:disabled={percentage === 5}
          on:click={event => decreasePercentage(event)}>
          <span class="material-icons">remove</span>
        </div>
        <h3>{`${percentage ? percentage : 5} % tip`}</h3>
        <div
          class="flex center button button-plus"
          class:disabled={percentage === 25}
          on:click={event => increasePercentage(event)}>
          <span class="material-icons">add</span>
        </div>

      </div>
      <h3>{`How did you like it at ${getRestaurant()}?`}</h3>
      <div class="description" in:fade>
        <span>{getDescription(percentage)}</span>
      </div>
    </div>
  </div>
</section>
