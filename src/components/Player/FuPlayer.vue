<template>
  <div class="fu-player">
        <img 
            v-bind:src="image_url" 
            :alt="name" 
            class="fu-player__image"
        >
        <div class="fu-player__details">
            <p class="fu-player__name">{{name}}</p>
            <p class="fu-player__album">{{album}}</p>
        </div>
        <div class="fu-player__botons">
            <div 
             class="fu-player__previous" 
             v-on:click="handlePreviousTrack"
             :disabled="current_clip_index == 0"
             :class="{'player__disabled_button': current_clip_index == 0}"
            >
                <i class="fas fa-step-backward"></i>
            </div>
            <div class="fu-player__play" v-on:click="handleTrack">
                <i v-show="isPaused" class="fas fa-play"></i>
                <i v-show="!isPaused" class="fas fa-pause"></i>
            </div>
            <div 
             class="fu-player__next" 
             v-on:click="handleNextTrack"
             :disabled="current_clip_index == playlist_length-1"
             :class="{'player__disabled_button': current_clip_index == playlist_length-1}"
            >
                <i class="fas fa-step-forward"></i>
            </div>
        </div>
        <div class="fu-player__volume">
            <input 
                class="fu-player__range" 
                type="range" 
                min="0" 
                max="100"
                v-model="volume"
                v-on:input="handleVolume"
            >
            <div class="fu-player__volumeIcon"
                v-on:click="toggleVolume"
            >
                <i class="fas fa-volume-up"
                    v-show="volume > 0"
                ></i>
                <i class="fas fa-volume-mute"
                    v-show="volume == 0"
                ></i>
            </div>                
        </div>
        <div 
         class="currentProgressBar"
         @click="handleChangeTimeProgress"
        >            
            <div class="currentProgress" :style="{ width: currentProgressBar + '% !important' }">
                <span class="fu_player__progress-tooltip">{{ Math.round(currentTime) | fancyTimeFormat }} - {{ Math.round(audioDuration) | fancyTimeFormat }}</span>
            </div>
        </div>
    </div> 
</template>

<script>
export default {
    name: "FuPlayer",
    props : 
    {
        name : 
        {
            type : String,
            required : true
        },
        album : 
        {
            type : String,
            required : true
        },
        url : 
        {
            type : String,
            required : true
        },
        image_url : 
        {
            type : String,
            required : true
        },
        current_clip_index :
        {
            type : Number,
            required : true
        },
        playlist_length:
        {
            type : Number,
            required : true
        },
    },
    data() 
    {
        return {
            audio : new Audio(this.url),
            isPaused: true,
            volume : 20,
            muted : false,
            currentTime : 0,
            currentProgressBar : 0,
            audioDuration : 0,
            checkingCurrentPositionInTrack: "",
        }
    },
    filters: 
    {
        fancyTimeFormat: function(s) {
            return (s - (s %= 60)) / 60 + (9 < s ? ":" : ":0") + s;
        }
    },
    methods :
    {
        handleTrack()
        {
            if(this.audio.paused)
            {
                this.audio.play();
                this.audio.volume = this.volume/100;
                this.isPaused = false;
                this.getCurrentTimeEverySecond(true);
            } else
            {
                this.audio.pause();
                this.isPaused = true;
                clearTimeout(this.checkingCurrentPositionInTrack);
            }

        },
        handleVolume()
        {
            this.audio.volume = this.volume/100;
        },
        toggleVolume()
        {
            if(this.audio.muted)
            {
                this.volume = 100;                
            } else 
            {
                this.volume = 0;
            }
            this.audio.muted = !this.audio.muted;
            this.audio.volume = this.volume/100;
        },
        getCurrentTimeEverySecond: function(startStop) 
        {
			var localThis = this;
			this.checkingCurrentPositionInTrack = setTimeout(
				function() {
					localThis.currentTime = localThis.audio.currentTime;					
                    localThis.audioDuration = isNaN(this.audio.duration) ? 0 : this.audio.duration;
                    if(!isNaN(localThis.audioDuration))
                    {
                        localThis.currentProgressBar = localThis.audio.currentTime / localThis.audioDuration * 100;                        
                    }                    

					localThis.getCurrentTimeEverySecond(true);
				}.bind(this),
				1000
			);
        },
        handleChangeTimeProgress(e)
        {
            let x_position          = e.clientX;
            let w_progress_bar      = document.getElementsByClassName("currentProgressBar")[0].offsetWidth;
            let progress_percent    = ( 100 * x_position ) / w_progress_bar;
            let seconds_to_change   = ( this.audioDuration * progress_percent ) / 100;

            this.audio.currentTime  = seconds_to_change;
        },
        handlePreviousTrack()
        {
            this.$emit('previous-track');
        },
        handleNextTrack()
        {
            this.$emit('next-track');
        },
        handleEnded()
        {
            if(this.current_clip_index + 1 == this.playlist_length)
            {                
                this.audio.pause();
                clearTimeout(this.checkingCurrentPositionInTrack);
                this.isPaused = true;
                this.currentTime = 0;
                this.currentProgressBar = 0;
                this.audioDuration = 0;
            } else
            {
                this.$emit('next-track');
            }
        },
    },    
    watch: 
    {        
        url : function(newUrl, oldUrl)
        {
            if(newUrl !== oldUrl)
            {
                this.audioDuration = 0;
                this.currentProgressBar = 0;
                this.checkingCurrentPositionInTrack = 0;
                this.currentTime = 0;
                this.audio.pause();
                this.isPaused = true;
                this.audio = new Audio(newUrl);                
            }
        },        
    },
    mounted()
    {
        this.audio.addEventListener("ended", this.handleEnded);        
    },
    updated()
    {
        this.audio.addEventListener("ended", this.handleEnded);        
    },
}     

// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
* 
{
    margin : 0;
}

.fu-player
{
    display: grid;
    grid-template-columns : 100px repeat(3, 1fr);
    background-color: #EB5757;
    width: 100%;
    height: 100px;
}

.fu-player__image
{
    width: 100px;
    height: 100px;
    object-fit: cover;
    vertical-align: top;
}

.fu-player__details
{
    margin-left: 20px;
    color: #fff;
    align-self: center;
}

.fu-player__name
{
    font-size: 14px;
    font-weight: bold;
}

.fu-player__album
{
    font-size: 12px;
    font-weight: 400;
}

.fu-player__botons
{
    display: flex;
    align-items: center;
    justify-content: center;
}

.fu-player__play
{
    align-self: center;
    justify-self: center;
    color: #fff;
    font-size: 20px;
    background-color: #FF7676;
    width: 60px;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
    cursor: pointer;
}

.fu-player__next
{
    padding-left: 20px;
}

.fu-player__previous
{
    padding-right: 20px;
}

.fu-player__next,
.fu-player__previous
{
    align-self: center;
    justify-self: center;
    color: #fff;
    font-size: 20px;
    width: 32px;
    height: 32px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}

.player__disabled_button
{
    color: rgba(255, 255, 255, 0.33);
	cursor: initial;
}

.fu-player__volume
{
    justify-self: end;
    display: flex;
    align-items: center;
    margin-right: 32px;
}

.fu-player__range
{
    appearance : none;
    outline: none;
    border-radius: 8px;
    height: 6px;
    cursor: pointer;
    
}

.fu-player__range::-webkit-slider-thumb 
{
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  background: #fff;
  border-radius: 50px;
  cursor: pointer;
}

.fu-player__volumeIcon
{
    color: #fff;
    font-size: 20px;
    margin-left: 40px;
}

.fa-volume-up,
.fa-volume-mute
{
    cursor: pointer;
}

.currentProgressBar
{
    width: 100%;
    height: 5px;
    background-color: rgba(94, 92, 92, 0.475);
    position: fixed;
    bottom: 100px;
    cursor: pointer;
    transition: 0.25s ease-in-out;
}
.currentProgress
{    
    height: 100%;
    background-color: #EB5757;
    cursor: pointer;
}

.currentProgressBar:hover
{    
    height: 20px;
}

.currentProgressBar:hover .fu_player__progress-tooltip
{    
    visibility: visible;
    opacity: 1;
}

.fu_player__progress-tooltip
{
    visibility: hidden;
    width: 120px;
    background-color: #FF7676;
    color: #fff;
    text-align: center;
    border-radius: 6px;
    padding: 5px 0;
    position: absolute;
    z-index: 1;
    bottom: 125%;
    left: 50%;
    margin-left: -60px;
    opacity: 0;
    transition: opacity 0.3s;
}

</style>