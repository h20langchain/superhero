<script>
  export let family;
</script>

<div class="family-info">
  <h2>Family Information</h2>

  <div class="info-item">
    <div class="label">Parents</div>
    <div class="value">{family.parents}</div>
  </div>

  <div class="info-item">
    <div class="label">Siblings</div>
    <div class="value">{family.siblings}</div>
  </div>

  <div class="info-item">
    <div class="label">Children</div>
    <div class="value">{family.children}</div>
  </div>
</div>

<style>
  .family-info {
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
</style>
