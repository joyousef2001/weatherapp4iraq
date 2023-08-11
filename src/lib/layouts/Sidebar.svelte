<!-- src/Sidebar.svelte -->
<script>
  import { translations } from "../translations/translations.js";

  export let cities = [];
  export let selectedCity = "";
  export let handleCitySelect = () => {};
</script>

<div class="sidebar card">
  {#each cities as city}
    <button
      class:active={selectedCity === city}
      on:click={(event) => handleCitySelect(event, city)}
    >
      {translations[city] || city}
    </button>
  {/each}
</div>

<style>
  /* Sidebar.scss */
  @media (max-width: 768px) {
  .sidebar{
    display: none;
  }
}

  .sidebar {
    position: fixed;
    top: 0;
    right: 0;
    height: 92vh;
    scrollbar-color: #666666 #e4e4e4;
    scrollbar-width: thin;
    direction: ltr;
    width: 20%;
    border: #e4e4e4 solid 2px;
    background-color: rgba(240, 240, 240, 0.2); /* Transparent background */
    margin: 20px;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    color: white;
    overflow-y: auto;
    transition: width 0.3s ease, background-color 0.3s ease;
    z-index: 100;
  }

  .sidebar:hover {
    width: 21%; /* Expand width on hover */
    background-color: rgba(
      240,
      240,
      240,
      0.3
    ); /* Lighter background on hover */
  }

  button {
    border-radius: 20px;
    display: block;
    width: 100%;
    padding: 10px;
    background-color: transparent;
    border: none;
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-size: 24px;
  }

  button.active {
    background-color: rgba(255, 255, 255, 0.1);
  }
</style>
