<script>
  import { onMount } from "svelte";
  import { fade, fly } from "svelte/transition";
  import HeroProfile from "./components/HeroProfile.svelte";
  import FamilyInfo from "./components/FamilyInfo.svelte";
  import Vehicles from "./components/Vehicles.svelte";
  import Properties from "./components/Properties.svelte";
  import FinancialStatus from "./components/FinancialStatus.svelte";
  import NetworkAnalysis from "./components/NetworkAnalysis.svelte";
  import { heroData } from "./lib/heroData.js";

  // Default to Batman as the active hero
  let activeHeroId = "Batman";
  let activeHero = heroData[activeHeroId];

  // Handle character selection from the network graph
  function handleCharacterSelect(event) {
    const selectedCharacterId = event.detail;
    if (heroData[selectedCharacterId]) {
      activeHeroId = selectedCharacterId;
      activeHero = heroData[activeHeroId];
    }
  }
</script>

<main>
  <header>
    <div class="nav">
      <span class="nav-item active">
        <i class="icon-database"></i> Hero Database
      </span>
      <span class="nav-item">
        <i class="icon-user"></i> Heroes
      </span>
    </div>
  </header>

  <div class="container">
    {#key activeHeroId}
      <div in:fly={{ y: 20, duration: 300 }} out:fade={{ duration: 200 }}>
        <HeroProfile hero={activeHero} />

        <div class="info-grid">
          <FamilyInfo family={activeHero.family} />
          <Vehicles vehicles={activeHero.vehicles} />
        </div>

        <div class="info-grid">
          <FinancialStatus financials={activeHero.financials} />
          <Properties properties={activeHero.properties} />
        </div>
      </div>
    {/key}

    <NetworkAnalysis
      relationships={activeHero.relationships}
      allCharacters={heroData}
      on:selectCharacter={handleCharacterSelect}
    />

    <footer>
      <p>Built by svt. The source code is available on GitHub.</p>
    </footer>
  </div>
</main>

<style>
  main {
    font-family: "Inter", sans-serif;
    background-color: #f8f9fa;
    min-height: 100vh;
  }

  header {
    background-color: white;
    border-bottom: 1px solid #e9ecef;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.04);
  }

  .nav {
    display: flex;
    padding: 0 2rem;
    height: 60px;
    align-items: center;
  }

  .nav-item {
    margin-right: 2rem;
    padding: 0.5rem 0;
    color: #6c757d;
    cursor: pointer;
    font-weight: 500;
    display: flex;
    align-items: center;
  }

  .nav-item.active {
    color: #212529;
    border-bottom: 2px solid #0d6efd;
  }

  .container {
    max-width: 1140px;
    margin: 0 auto;
    padding: 2rem;
  }

  .info-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  footer {
    text-align: center;
    color: #6c757d;
    padding: 1rem 0;
    margin-top: 2rem;
    border-top: 1px solid #e9ecef;
  }

  @media (max-width: 768px) {
    .info-grid {
      grid-template-columns: 1fr;
    }
  }
</style>
