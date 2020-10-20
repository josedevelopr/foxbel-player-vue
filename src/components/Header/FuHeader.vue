<template>
  <div class="FuHeader">
      <fu-search-bar/>
      <fu-user-session class="header__user_section"/>
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
}
</script>

<style>
    .FuHeader
    {
        width: 100%;
        padding-left: 40px;
        padding-top: 30px;
        display: flex;
    }

    .header__user_section
    {
        position: absolute;
        right: 0;        
    }
</style>