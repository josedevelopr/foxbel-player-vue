<template>
    <div class="FuNavbar">        
        <div class="navbar__top">
            <div class="navbar__logo">
                <img :src="logo" alt="logo_foxbel_app">
            </div>
            <div class="navbar__library_section">
                <div class="navbar__item navbar__title">
                    <span>Mi librería</span>
                </div>
                <div class="navbar__item">
                    <a>Recientes</a>
                </div>
                <div class="navbar__item">
                    <a 
                     :class="current_menu_selected === 'artistTracks' ? 'item__active' : ''"
                     href="#"
                     @click="handleMenuSelected('artistTracks')"
                    >Artistas</a>
                </div>
                <div class="navbar__item">
                    <a
                    :class="current_menu_selected === 'albumTracks' ? 'item__active' : ''"
                     href="#"
                     @click="handleMenuSelected('albumTracks')"
                    >Álbums</a>
                </div>
                <div class="navbar__item">
                    <a :class="current_menu_selected === 'playListTracks' ? 'item__active' : ''"
                     href="#"
                     @click="handleMenuSelected('playListTracks')">Canciones</a>
                </div>
                <div class="navbar__item">
                    <a href="#">Estaciones</a>
                </div>
            </div>
            <div class="navbar__playlist_section">
                <div class="navbar__item navbar__title">
                    <span>Playlist</span>
                </div>
                <div class="navbar__item">
                    <a href="#">Metal</a>
                </div>
                <div class="navbar__item">
                    <a href="#">Para bailar</a>
                </div>
                <div class="navbar__item">
                    <a href="#">Rock 90's</a>
                </div>
                <div class="navbar__item">
                    <a href="#">Baladas</a>
                </div>                
            </div>            
        </div>
    </div>
</template>

<script>
import logo from '../../assets/foxbel-music.png';
export default {
    name : 'NavBar',  
    data()
    {
        return{
            logo : logo,
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
        handleMenuSelected(menuSelected)
        {
            this.$emit('handle_menu_selected', menuSelected);
        },
    },
}
// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@300&display=swap');
    .FuNavbar
    {
        box-sizing: border-box;
        grid-row: 1/2;
        grid-column: 1/2;
        height: 100vh;
        width: 330px;
        top: 0;
        left: 0;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        overflow: hidden;
        background: #662323;
        color: #cdd7d6;        
    }

    .navbar__top
    {
        display: block !important;        
    }
    
    .navbar__logo
    {
        box-sizing: border-box;
        padding: 50px 40px 30px;        
    }
    .navbar__logo img
    {
        width: 250px;
    }

    .navbar__item
    {
        padding: 3px 2vw;
        text-transform: capitalize;
        padding-left: 40px;
    }

    .navbar__item > a
    {
        display: inline-block;
        text-align: start;
        height: auto;
        /* Our sidebar item styling */
        text-decoration: none;
        color: #d1d1d1;
        transition: color 0.2s;
        padding-top: 8px;
    }

    .navbar__title span
    {
        font-family: 'Quicksand', sans-serif;
        font-size: 22px;
        font-weight: bold;
        color: #fff;
    }

    .navbar__item a:hover
    {
        color: #E86060;
    }

    .navbar__library_section
    {
        padding-bottom: 30px;
    }

    .item__active
    {
        border-left: 4px solid #E86060;
        margin-left: -36px;
        padding-left: 36px;        
    }
    .item__active a
    {
        color: #E86060;
        font-family: 'Quicksand', sans-serif;
        font-size: 16px;
        font-weight: bold;
    }
    
</style>