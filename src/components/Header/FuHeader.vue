<template>
  <div class="FuHeader">
      <fu-search-bar 
       class="FuSearchBar"
       :current_menu_selected="current_menu_selected"
       @get-search="getSearch"
       />
      <fu-user-session class="FuUserSession"/>
  </div> 
</template>

<script>
import FuSearchBar from '../SearchBar/FuSearchBar'
import FuUserSession from '../User/FuUserSession'

export default {
    name: 'FuHeader',
    components: 
    {
        FuSearchBar,
        FuUserSession,
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
        getSearch(search, type)
        {
            this.$emit('get-search', search, type);
        },
    },
}
</script>

<style>
    .FuHeader
    {
        width: 100%;
        padding-left: 40px;
        padding-top: 30px;
        display: flex;
        box-sizing: border-box;
    }

    .FuSearchBar
    {
        align-self: flex-start;  
        margin-right: 249px;
    }

    .FuUserSession
    {
        align-self: flex-end;              
        display:flex;
        justify-content: flex-end;
    }
</style>