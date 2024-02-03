<template >
<div class="card">
    <div class="content-wrapper">
      <img src="../assets/typebot.jpeg" alt="Logo" class="rounded-logo">
      <div class="text-section">
          <p class="sub-text gray-text">👽 Trabalho</p>
          <slot class="main-text">Iago Jorge</slot>
        <p class="sub-text sub-custom">☠️ teste     👻teste</p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
    name:"Card "
    
}
</script>
<style scoped>
    .card{
        background: #22272b;
        border-radius: 0.4rem;
        box-shadow: 0 1 px 0 rgba(9, 30, 66, 0.25);
        font-size: 1.6rem;
        margin-bottom: 0.8rem;
        cursor: pointer;
    }

  .content-wrapper {
    display: flex;
    align-items: center;
  }

  .rounded-logo {
    margin-left: 10px;
    width: 40px; 
    height: 40px; 
    border-radius: 50%;
    margin-right: 10px; 
  }

  .text-section {
    flex: 1;
  }

  .main-text {
    font-size: 14px; 
    font-weight: bold;
    margin: 0;
  }

  .sub-text {
    font-size: 11px; 
    margin: 5px 0;
  }

  .gray-text {
    color: #888; 
  }

  
</style>