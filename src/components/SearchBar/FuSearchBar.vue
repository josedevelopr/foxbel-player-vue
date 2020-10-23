<template>
  <div class="FuSearchBar">
      <input        
       v-model="search"
       type="text" placeholder="Buscar" class="search__input">
      <i 
       class="fas fa-search"       
       @click="getSearch"
       ></i>
  </div>
</template>

<script>
export default 
{
    name : 'FuSearchBar',
    data()
    {
        return{
            search : ''
        }
    },
    props :
    {
        current_menu_selected :
        {
            type : String,
            required : true
        },
    },    
    methods:
    {
        getSearch()
        {            
            this.$emit('get-search', this.search, this.current_menu_selected);
            this.search = '';
        },
    },
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .FuSearchBar
    {
        box-sizing: border-box;
        height: 36px;
        width: 524px;   
        display: flex;    
        align-items: center;
        justify-content: center;
    }

    .search__input
    {
        width: 100%;
        height: 100%;
        border-radius: 30px;
        outline: none;
        padding-left: 16px;
        border: 2px solid rgb(135, 137, 138);
        color: rgba(97, 97, 97, 0.897);
        font-family: 'Quicksand', sans-serif;
        font-size: 18px;
        font-weight: normal;    
    }

    .search__input::placeholder
    {
        color: rgba(97, 97, 97, 0.897);
        font-family: 'Quicksand', sans-serif;
        font-size: 18px;
        font-weight: normal;
    }

    .fa-search
    {
        margin-left: -40px;
        cursor: pointer;
        color: #BDBDBD;
    }

    .hide__element
    {
        display: none;
    }
</style>