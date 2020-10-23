<template>
    <div class="FuMain">
        <fu-header 
         :current_menu_selected="current_menu_selected"
         @get-search="getSearch"
        />
        <main-artist-result 
         v-show="itHasMainArtistResult"
         :clips="clips"
         :current_clip_id="current_clip_id"
         :current_menu_selected="current_menu_selected"
         @handle-click-tracks="handleClickTracks"
        />
        <search-result 
         v-if="current_menu_selected ==='playListTracks' "
         :clips="clips"
         :current_clip_id="current_clip_id"
         :current_menu_selected="current_menu_selected"
         @handle-click-tracks="handleClickTracks"
        />
        <fu-album 
         v-else-if="current_menu_selected === 'albumTracks'"
         :clips="clips"
         :current_clip_id="current_clip_id"
         :current_menu_selected="current_menu_selected"
         @handle-click-tracks="handleClickTracks"
        />
        <fu-artist 
         v-else
         :clips="clips"
         :current_clip_id="current_clip_id"
         :current_menu_selected="current_menu_selected"
         @handle-click-tracks="handleClickTracks"
        />
    </div>
</template>

<script>
import Vue from 'vue'
import VueRouter from 'vue-router'

import FuHeader from '../Header/FuHeader'
import MainArtistResult from '../Result/MainArtistResult'
import SearchResult from '../Result/SearchResult'
import FuAlbum from '../Album/FuAlbum'
import FuArtist from '../Artist/FuArtist'

Vue.use(VueRouter);

const routes = 
[
    {path:'/', component : SearchResult},
    {path:'/artistas', component : FuArtist},
    {path:'/albums', component : FuAlbum},
]

export default {
    namne: 'FuMain',
    components: 
    {
       FuHeader, 
       MainArtistResult,
       SearchResult,
       FuAlbum,
       FuArtist,
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
            itHasMainArtistResult : false,
        }
    },
    methods:
    {
        handleClickTracks(item, type)
        {
            this.$emit('handle-click-tracks', item, type);
        },
        getSearch(search, type)
        {
            this.$emit('get-search', search, type);
        },
    },
}
// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
    .FuMain
    {
        box-sizing: border-box;
        grid-row: 1/2;
        grid-column: 1/2;
        height: 100vh;
        width: calc(100% - 330px);
        overflow-y: scroll;
    }

    .FuMain::-webkit-scrollbar
    {
        width: 13px;
    }
    .FuMain::-webkit-scrollbar-track 
    {
        background: rgb(206, 205, 205);        /* color of the tracking area */
    }
    .FuMain::-webkit-scrollbar-thumb 
    {
        background-color: rgb(155, 154, 154);    /* color of the scroll thumb */
        border-radius: 30px;       /* roundness of the scroll thumb */
        border: 3px solid rgb(206, 205, 205);        /* color of the tracking area */;  /* creates padding around scroll thumb */
    }
</style>