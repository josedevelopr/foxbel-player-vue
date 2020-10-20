<template>
  <div class="FuUserSession">
      <i class="fas fa-user"></i>
      <span class="user__name">Jose A.</span>
  </div>
</template>

<script>
export default 
{
    name : 'FuUserSession'
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .FuUserSession
    {
        width: 114px;
        height: 22px;        
    }

    .user__name
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 16px;
        font-weight: normal;
        padding-left: 12px;
        cursor: pointer;
    }

    .fa-user
    {
        cursor: pointer;
        color: #E86060;
    }
</style>