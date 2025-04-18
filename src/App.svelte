<script>
  import { onMount } from "svelte";
  import HeroProfile from "./components/HeroProfile.svelte";
  import FamilyInfo from "./components/FamilyInfo.svelte";
  import Vehicles from "./components/Vehicles.svelte";
  import Properties from "./components/Properties.svelte";
  import FinancialStatus from "./components/FinancialStatus.svelte";
  import NetworkAnalysis from "./components/NetworkAnalysis.svelte";

  let activeHero = {
    name: "Bruce Wayne",
    alias: "Batman",
    age: 35,
    location: "Gotham City",
    occupation: "CEO of Wayne Enterprises",
    profileImg: "/hero-profile.svg",
    family: {
      parents: "Thomas & Martha Wayne (Deceased)",
      siblings: "None",
      children: "Damian Wayne (Biological), Dick Grayson (Adopted)",
    },
    financials: {
      netWorth: "$9.2B",
      annualIncome: "$750M",
      majorAssets: ["Wayne Manor", "Batcave", "Wayne Tower"],
    },
    vehicles: ["Batmobile", "Batwing", "Batpod"],
    properties: ["Wayne Manor", "Wayne Tower", "Various Global Properties"],
    relationships: {
      allies: ["Robin", "Alfred", "Gordon", "Nightwing"],
      enemies: ["Joker", "Riddler", "Penguin", "Two-Face"],
    },
  };
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
    <HeroProfile hero={activeHero} />

    <div class="info-grid">
      <FamilyInfo family={activeHero.family} />
      <Vehicles vehicles={activeHero.vehicles} />
    </div>

    <div class="info-grid">
      <FinancialStatus financials={activeHero.financials} />
      <Properties properties={activeHero.properties} />
    </div>

    <NetworkAnalysis relationships={activeHero.relationships} />

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
