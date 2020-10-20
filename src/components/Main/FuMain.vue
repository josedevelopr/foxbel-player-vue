<template>
    <div class="FuMain">
        <fu-header />        
    </div>
</template>

<script>
import FuHeader from '../Header/FuHeader'

export default {
    namne: 'FuMain',
    components: 
    {
       FuHeader, 
    }
}
</script>

<style>
    .FuMain
    {
        box-sizing: border-box;
        grid-row: 1/2;
        grid-column: 1/2;
        height: 100vh;
        width: calc(100% - 330px);        
        overflow: hidden;        
    }
</style>