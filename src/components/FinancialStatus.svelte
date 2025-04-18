<script>
  export let financials;
</script>

<div class="financial-status">
  <h2>Financial Status</h2>

  <div class="info-item">
    <div class="label">Net Worth</div>
    <div class="value">{financials.netWorth}</div>
  </div>

  <div class="info-item">
    <div class="label">Annual Income</div>
    <div class="value">{financials.annualIncome}</div>
  </div>

  <div class="info-item">
    <div class="label">Major Assets</div>
    <ul class="asset-list">
      {#each financials.majorAssets as asset}
        <li>
          • {asset}
        </li>
      {/each}
    </ul>
  </div>
</div>

<style>
  .financial-status {
    background-color: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    padding: 1.5rem;
    height: 100%;
  }

  h2 {
    margin-top: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: #343a40;
    margin-bottom: 1.5rem;
  }

  .info-item {
    margin-bottom: 1.25rem;
  }

  .info-item:last-child {
    margin-bottom: 0;
  }

  .label {
    font-size: 0.875rem;
    color: #6c757d;
    margin-bottom: 0.25rem;
  }

  .value {
    font-weight: 500;
    color: #212529;
  }

  .asset-list {
    list-style: none;
    padding: 0;
    margin: 0.25rem 0 0 0;
  }

  .asset-list li {
    margin-bottom: 0.5rem;
    color: #212529;
  }
</style>
