<template>
  <div class="FuSearchBar">
      <input type="text" placeholder="Buscar" class="search__input">
  </div>
</template>

<script>
export default 
{
    name : 'FuSearchBar',
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .FuSearchBar
    {
        height: 36px;
        width: 524px;        
    }

    .search__input
    {
        width: 100%;
        height: 100%;
        border-radius: 30px;
        outline: none;
        padding-left: 16px;
        border: 2px solid rgb(135, 137, 138);        
    }

    .search__input::placeholder
    {
        color: rgba(97, 97, 97, 0.897);
        font-family: 'Quicksand', sans-serif;
        font-size: 18px;
        font-weight: normal;
    }
</style>