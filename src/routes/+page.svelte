<script lang="ts">
  import Fa from 'svelte-fa'
  import { faGithub } from '@fortawesome/free-brands-svg-icons'

  import Blanchor from '$lib/Blanchor.svelte';
	import { onMount } from 'svelte';

  let totalGramsFlour = $state(250)
  let totalHydration = $state(70)
  let starterPercent = $state(20)
  let starterHydration = $state(100)
  let saltPercent = $state(2.3)
  let ready = $state(false);

  let starterWeight = $derived(totalGramsFlour * starterPercent / 100)
  let starterFlourWeight = $derived(100 * starterWeight / (100 + starterHydration))
  let starterWaterWeight = $derived(starterWeight * starterHydration / (100 + starterHydration))

  let outputGramsFlourToAdd = $derived(totalGramsFlour - starterFlourWeight);
  let outputGramsWaterToAdd = $derived(totalGramsFlour * totalHydration / 100 - starterWaterWeight);
  let outputStarterToAdd = $derived(totalGramsFlour * starterPercent / 100);
  let outputSaltToAdd = $derived(totalGramsFlour * saltPercent / 100);

  onMount(() => {
    if (typeof localStorage !== 'undefined') {
      totalGramsFlour =  parseInt(localStorage.getItem("totalGramsFlour") || "")  || totalGramsFlour
      totalHydration =   parseInt(localStorage.getItem("totalHydration") || "")   || totalHydration
      starterPercent =   parseInt(localStorage.getItem("starterPercent") || "")   || starterPercent
      starterHydration = parseInt(localStorage.getItem("starterHydration") || "") || starterHydration
      saltPercent =      parseFloat(localStorage.getItem("saltPercent") || "")      || saltPercent

      ready = true
    }
  });

  $effect(() => {ready && localStorage.setItem("totalGramsFlour", totalGramsFlour.toString())});
  $effect(() => {ready && localStorage.setItem("totalHydration",  totalHydration.toString())});
  $effect(() => {ready && localStorage.setItem("starterPercent",  starterPercent.toString())});
  $effect(() => {ready && localStorage.setItem("starterHydration",starterHydration.toString())});
  $effect(() => {ready && localStorage.setItem("saltPercent",     saltPercent.toString())});
</script>

<main>
  <section>
    <div>
      <h1>🍞 Sourdough Ratios Calculator</h1>

      <div id="main-flex">
        <div>
          <h2>Ratios</h2>

          <div class="row">
            <label for="total-grams-flour">🌾 Total Grams of Flour</label>
            <input id="total-grams-flour" type="number" bind:value={totalGramsFlour} step="1" min=0 max=100000/>
          </div>

          <div class="row" style="margin-bottom:0;">
            <label for="hydration">💧 Total Hydration %</label>
            <input id="hydration" type="number" bind:value={totalHydration} step="1" min=50 max=100/>
          </div>
          <p style="margin-top:0">(I like <button class="hydration-suggestion" onclick={() => totalHydration = 70}>70%</button> for AP, <button class="hydration-suggestion" onclick={() => totalHydration = 85}>85%</button> for whole wheat)</p>

          <div class="row">
            <label for="hydration">🫙 Starter %</label>
            <input id="hydration" type="number" bind:value={starterPercent} step="1" min=1 max=99/>
          </div>

          <div class="row">
            <label for="hydration">💧 Starter Hydration %</label>
            <input id="hydration" type="number" bind:value={starterHydration} step="1" min=25 max=200/>
          </div>

          <div class="row">
            <label for="hydration">🧂 Salt %</label>
            <input id="hydration" type="number" bind:value={saltPercent} step="0.1" min=0 max=30/>
          </div>
        </div>

        <div>
          <h2>Recipe</h2>

          <p><b>Step 1: Hydration</b></p>
          <p class="flex"><span>🌾 Flour to add:</span> <b>{Math.round(outputGramsFlourToAdd)}g</b></p>
          <p class="flex"><span>💧 Water to add:</span> <b>{Math.round(outputGramsWaterToAdd)}g</b></p>

          <p><b>Step 2: Starter and Salt</b></p>
          <p class="flex"><span>🫙 Starter to add:</span> <b>{Math.round(outputStarterToAdd)}g</b></p>
          <p class="flex"><span>🧂 Salt to add:</span> <b>{Math.round(outputSaltToAdd)}g</b></p>
        </div>
      </div>
    </div>
  </section>
</main>

<style lang="scss">
  #main-flex {
    display: flex;
    flex-direction: column;

    @media only screen and (min-width: 600px) {
      flex-direction: row;

      &> div {
        width: 50%;

        &:first-child {
          padding-right: 1rem;
        }
        &:last-child {
          padding-left: 1rem;
        }
      }
    }
  }

  .row {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.5rem;

    label {
      margin-right: 0.5rem;
    }

    input {
      margin-bottom: 0;
    }
  }

  label {
    font-weight: bold;
  }

  .hydration-suggestion {
    cursor: pointer;
    text-decoration: underline;
    color: rgb(0,100,200);
    background: none;
  }

  p.flex {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
</style>