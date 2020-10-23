<template>
  <div class="SearchResult">
      <h1 class="result__title">Albums</h1>
      <div class="result__container">
        <result-item 
         v-for="(item, key) in clips"
         :key="`item_result${key}`"
         :name="item.title"
         :artist_name="item.artist.name"
         :cover_url="item.cover_medium ? item.cover_medium : item.album.cover_medium"
         :item="item"
         :current_clip_id="current_clip_id"
         :current_menu_selected="current_menu_selected"
         @handle-click-tracks="handleClickTracks"
        />
      </div>      
  </div>
</template>

<script>
import ResultItem from '../Result/ResultItem'

export default {
    namne : 'FuAlbum',   
    components: 
    {
        ResultItem,
    },
    props :
    {
        clips : 
        {
            type : Array,
            required : true      
        },
        current_clip_id :
        {
            type : Number,
            required : true
        },
        current_menu_selected :
        {
            type : String,
            required : true
        },
    },
    data()
    {
        return{

        }
    },
    methods:
    {
        handleClickTracks(id, type)
        {
            this.$emit('handle-click-tracks', id, type);
        },
    },
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .SearchResult
    {
        padding-top: 40px;
        padding-left: 40px;
        min-height: 100vh;
    }
    .result__title
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 22px;
        font-weight: bold;
        color: #E86060;
        margin-bottom: 20px;
    }

    .result__container
    {
        display: flex;
        flex-flow: row wrap;
        justify-content: flex-start;
    }

    .result__container .ResultItem
    {
        margin-right: 20px;
        margin-bottom: 20px;
    }
</style>