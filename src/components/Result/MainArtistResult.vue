<template>
  <div class="MainArtistResult">
      <div class="artist__img">
          <i class="fas fa-play button__play"></i>
      </div>
      <div class="artist__info">
          <div class="artist__data">
              <h1 class="artist__name">
                  Adele 21
              </h1>
              <p class="data__description1">Lo mejor de Adele <span class="data__followers_number">321, 123 seguidores</span>   </p>
              <p class="data__description2">
                Descripcion 2
              </p>

          </div>
          <div class="artist__buttons">
            <button class="button btn__primary">Reproducir</button>
            <button class="button btn__secondary">Seguir</button>
            <button class="btn__icon"><i class="fas fa-ellipsis-h"></i></button>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    name : 'MainArtistResult',
}
// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
    .MainArtistResult
    {
        width: 890px;
        height: 250px;        
        margin: 40px auto 0;
        margin-left:40px ;
        display: flex;
        background-image: url("../../assets/adele-wallpaper.png");       
        background-size: cover;                
        box-shadow: inset 0 0 0 1000px #e21c1c8c;
    }

    .artist__img
    {
        min-width: 250px;
        min-height: 250px;
        background-image: url("../../assets/albun-portada.png");
        background-repeat: no-repeat;
        background-size: 100% 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .button__play
    {   
        font-size: 72px;
        color: #fff;
        cursor: pointer;
    }
    .artist__info
    {
        padding-left: 30px;
        padding-top: 40px;
        padding-bottom: 22px;
        background-color: rgba(230, 230, 230, 0.446);        
        width: 100%;
    }
    .artist__data
    {
        height: auto;
        width: auto;
        color: #fff;
    }

    .artist__name
    {
        margin-bottom: 20px;
    }

    .artist__buttons
    {
        margin-top: 58px;
    }

    .artist__buttons button
    {
        margin-right: 20px;
    }
    .button
    {
        width: 120px;
        height: 31px;
        border-radius: 50px;
        box-sizing: border-box;        
        padding: 6px 22px 7px;        
        border: none;
        cursor: pointer;
    }
    .btn__primary
    {
        background-color: #E86060;
        color: #fff;
    }

    .btn__secondary
    {
        background-color: #ffffff00;
        color: #E86060;
        border: 1px #E86060 solid;
    }

    .btn__icon
    {
        height: 31px;
        background-color: #ffffff00;
        border: none;
        cursor: pointer;
        color: #fff;
        font-size: 18px;
    }
</style>