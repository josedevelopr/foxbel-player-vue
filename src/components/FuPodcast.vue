<template>
  <div 
   class="podcast"
   :class="['podcast', {
       'podcast--active' : active
   }]"
   >
      <img class="podcast__img" :alt="name" :src="image" >
      <p class="podcast__name">{{name}}</p>
  </div>
</template>

<script>
export default {
    name : 'FuPodcast',
    props :
    {
        image :
        {
            type : String,
            required : true
        },
        name :
        {
            type : String,
            required : true
        },
        active: {
            type : Boolean,
            default : false,
            required : false
        }
    }
}

// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
    .podcast
    {
        width: 145px;
        text-align: center;
    }
    
    .podcast__img
    {
        width: 144px;
        height: 164px;
        object-fit: cover;  
    }

    .podcast__name
    {
        margin: 0;
        margin-top:4px;
        font-weight: bold;
        font-size: 14px;
    }

    .podcast--active
    {
        border: 5px solid blue;
    }
</style>