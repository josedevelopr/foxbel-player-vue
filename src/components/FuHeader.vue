<template>
  <div class="header">
      <img class="header__logo" :src="logo" :alt="logo">
  </div>
</template>

<script>
import logo from '../assets/logo.png';

export default {
    name: 'FuHeader',
    data(){
        return{
            logo : logo,
        }
    },
}

// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
    .header
    {
        width: 100%;
        height: 70px;
        background-color: #2196F3;        
        display: flex;
        justify-content: center;
        align-items: center;
        position: fixed;
        top: 0;
    }
    .header__logo
    {  
        width: 150px;
        height: 42px;
    }
</style>