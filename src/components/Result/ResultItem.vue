<template>
  <div 
   class="ResultItem"
   :class="['compareClipWithCurrent', {
       'track--active' : active
   }]"
  >
      <div 
       class="item__cover"
       v-bind:style="{ 'background-image': 'url(' + cover_url + ')' }"
      >
          <i class="fas fa-ellipsis-v"></i>
          <i 
           class="fas fa-play item__button_play"
           @click="handleClickTracks(item, current_menu_selected)"
          ></i>
      </div>
      <div class="item__info">
          <h1 class="item__name">{{name}}</h1>
          <p class="item__artist_name">{{artist_name}}</p>
      </div>

  </div>
</template>

<script>
export default {
    name: 'ResultItem',
    props :
    {
        name :
        {
            type : String,
            required : true
        },
        artist_name :
        {
            type : String,
            required : true
        },
        cover_url:
        {
            type : String,
            required : true
        },
        item:
        {
            type : Object,
            required : true
        },
        current_clip_id :
        {
            type : Number,
            required : true
        },
        active: 
        {
            type : Boolean,
            default : false,
            required : false
        },
        current_menu_selected :
        {
            type : String,
            required : true
        },
    },
    methods :
    {
        handleClickTracks(id, type)
        {
            this.$emit('handle-click-tracks', id, type);
        },
    },
    computed : 
    {
        compareClipWithCurrent()
        {
            return this.current_clip_id == this.item.id;            
        },
    },
}
</script>
    
<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .ResultItem
    {
        width: 160px;
        height: 206px;
    }
    
    .item__cover
    {
        width: 160px;
        height: 160px;
        /* background-image: url("../../assets/albun-portada.png"); */
        background-repeat: no-repeat;
        background-size: 100% 100%;
        display: flex;
        flex-direction: column;
        transition: opacity .1s ease-in-out;
    }
    
    .item__cover:hover
    {
        opacity: 0.8;
    }

    .item__info
    {
        width: 160px;
        height: calc(100% - 160px);
        padding-top: 8px;
    }
    .item__name
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 14px;
        font-weight: bold;
        color: #555555;
        cursor: pointer;
    }

    .item__artist_name
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 12px;
        font-weight: normal;
        color: #828282;
        cursor: pointer;
    }     
    .fa-ellipsis-v
    {
        color: #fff;
        font-size: 18px;
        cursor: pointer;
        align-self: flex-end;
        padding-top: 8px;
        padding-right: 8px;
    }

    .item__button_play
    {
        color: #fff;
        font-size: 32px;
        cursor: pointer;
        align-self: center;
        padding-top: 36px;
        transition: transform .1s;
    }
    
    .item__button_play:hover
    {
        transform: scale(1.1);
    }

    .track--active
    {
        border: 5px solid blue;
    }
</style>