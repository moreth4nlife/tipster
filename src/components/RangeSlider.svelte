<script>
  import { afterUpdate, createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  let element;
  let value = 0;

  const onInput = event => {
    if (element.classList.contains("animation-stop")) {
      element.classList.remove("animation-stop");
    }
    element.classList.add("animate");
  };

  const onMouseout = event => {
    element.classList.add("animation-stop");
  };

  afterUpdate(() => {
    dispatch("input", value);
  });

  export { value };
</script>

<style>
  .slider {
    background: url("/images/range.svg") repeat-x;
    background-size: contain;
    width: 100%;
    height: 30px;
    outline: none;
    opacity: 0.7;
    transition: opacity 0.5s;
    appearance: none;
    -webkit-transition: 0.5s;
    -webkit-appearance: none;
  }
  .slider::-webkit-slider-thumb {
    width: 7px;
    height: 30px;
    border-radius: 3px;
    background: #3be9d7;
    cursor: pointer;
    appearance: none;
    -webkit-appearance: none;
    transition: all 0.5s ease-in-out;
  }
  .slider:hover.slider::-webkit-slider-thumb {
    transform: scale(1.5);
  }
  :global(.animate) {
    animation: slider 3s linear infinite;
    animation-play-state: running;
  }
  :global(.animation-stop) {
    animation-play-state: paused;
  }
  @keyframes slider {
    0% {
      background-position: 0 0;
    }
    100% {
      background-position: -300px 0;
    }
  }
</style>

<input
  type="range"
  min="5"
  max="25"
  step="5"
  class="slider"
  bind:value
  bind:this={element}
  on:input={onInput}
  on:mouseout={onMouseout} />

<datalist id="steplist">
  <option>5</option>
  <option>10</option>
  <option>15</option>
  <option>20</option>
  <option>25</option>
</datalist>
