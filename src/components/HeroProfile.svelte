<script>
  export let hero;
</script>

<div class="hero-profile">
  <h2>Hero Profile</h2>

  <div class="profile-content">
    <div class="profile-image">
      <img src={hero.profileImg} alt={hero.name} />
    </div>

    <div class="profile-info">
      <h1>{hero.name}</h1>
      <p class="alias">Alias: {hero.alias}</p>

      <div class="info-grid">
        <div class="info-item">
          <div class="label">Age</div>
          <div class="value">{hero.age}</div>
        </div>

        <div class="info-item">
          <div class="label">Location</div>
          <div class="value">{hero.location}</div>
        </div>
      </div>

      <div class="info-item occupation">
        <div class="label">Occupation</div>
        <div class="value">{hero.occupation}</div>
      </div>
    </div>
  </div>
</div>

<style>
  .hero-profile {
    background-color: white;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
    margin-bottom: 1rem;
    padding: 1.5rem;
  }

  h2 {
    margin-top: 0;
    font-size: 1.25rem;
    font-weight: 600;
    color: #343a40;
    margin-bottom: 1rem;
  }

  .profile-content {
    display: flex;
    align-items: flex-start;
  }

  .profile-image {
    flex: 0 0 100px;
    margin-right: 1.5rem;
  }

  .profile-image img {
    width: 100%;
    height: auto;
    border-radius: 50%;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }

  .profile-info {
    flex: 1;
  }

  .profile-info h1 {
    margin-top: 0;
    font-size: 1.75rem;
    color: #212529;
    margin-bottom: 0.5rem;
  }

  .alias {
    color: #6c757d;
    margin-bottom: 1rem;
  }

  .info-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .info-item {
    margin-bottom: 0.75rem;
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

  @media (max-width: 576px) {
    .profile-content {
      flex-direction: column;
    }

    .profile-image {
      margin-right: 0;
      margin-bottom: 1rem;
      text-align: center;
    }

    .profile-image img {
      width: 80px;
    }
  }
</style>
