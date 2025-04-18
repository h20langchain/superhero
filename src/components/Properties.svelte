<script>
  export let properties;
</script>

<div class="properties">
  <h2>
    <i class="icon-building"></i> Properties
  </h2>

  <ul class="property-list">
    {#each properties as property}
      <li>
        <span class="bullet">•</span>
        {property}
      </li>
    {/each}
  </ul>
</div>

<style>
  .properties {
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
    display: flex;
    align-items: center;
  }

  h2 i {
    margin-right: 0.5rem;
    color: #0d6efd;
  }

  .property-list {
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .property-list li {
    margin-bottom: 0.75rem;
    display: flex;
    align-items: center;
  }

  .bullet {
    color: #0d6efd;
    margin-right: 0.5rem;
    font-size: 1.25rem;
    line-height: 1;
  }
</style>
