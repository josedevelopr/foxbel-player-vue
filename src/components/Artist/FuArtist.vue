<template>
    <div class="FuArtist">
        <h1 class="section__title">Artistas</h1>
        <div class="artist__container">
            <artist-result-item 
             v-for="(item, key) in clips"
             :key="`item_result${key}`"             
             :artist_name="item.name"
             :cover_url="item.picture_medium"
             :item="item"
             :current_clip_id="current_clip_id"
             :current_menu_selected="current_menu_selected"
             @handle-click-tracks="handleClickTracks"
            />
        </div>
    </div>
</template>

<script>
import ArtistResultItem from '../Artist/ArtistResultItem'

export default {
    name : 'FuArtist',
    components: 
    {
        ArtistResultItem,
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
    .FuArtist
    {
        padding-top: 40px;
        padding-left: 40px;
        min-height: 100vh;
    }
    .section__title
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 22px;
        font-weight: bold;
        color: #E86060;
        margin-bottom: 20px;
    }

    .artist__container
    {
        display: flex;
        flex-flow: row wrap;
        justify-content: flex-start;
    }

    .artist__container .ResultItem
    {
        margin-right: 20px;
        margin-bottom: 20px;
    }
</style>