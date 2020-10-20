<template>
  <div id="app">
    <fu-navbar/>
    <fu-main/>
    <!-- <div class="app-podcasts">
      <fu-podcast
        class="app-podcast"
        v-for="(podcast, key) in podcasts"
        :key="`podcast_${key}`"
        @click.native="handleClickPodcast(podcast)"
        :image="podcast.urls.banner_image.original"
        :name="podcast.title"   
        :active="comparePodcastWithCurrent(podcast)"     
      />
    </div>
    <div 
     class="app-player"
     v-if="currentPodcastClips.length > 0"
    >
      <fu-player 
      :name="getClip.title"
      :url="getClip.urls.high_mp3"
      :image_url="getClip.channel.urls.logo_image.original"
      :album="getClip.channel.title"
      :podcast_index="currentIndexPodcast"
      :playlist_length="currentPodcastClips.length"
      @next-track="handleNextTrack"
      @previous-track="handlePreviousTrack"
      />
    </div> -->
  </div>  
</template>

<script>
import FuHeader from './components/FuHeader'
import FuPlayer from './components/Player/FuPlayer'
import FuPodcast from './components/FuPodcast'
import FuNavbar from './components/Navbar/FuNavbar'
import FuMain from './components/Main/FuMain'

export default {
  name: 'App',
  data(){
    return {     
      podcasts : [],
      currentPodcastClips : [],
      currentPodcast : null,
      currentIndexPodcast : 0,
    }
  },
  methods:
  {
    async handleClickPodcast(podcast)
    {
      const {id} = podcast;
      this.currentPodcast = podcast;
      const {audio_clips} = await this.getAudioClip(id);
      this.currentPodcastClips = audio_clips;
      this.currentIndexPodcast = 0;
    },
    getAudioClip(id)
    {
      return fetch(`https://api.audioboom.com/channels/${id}/audio_clips`)
              .then((response) => response.json())
              .then((data) => data.body);
      
    },
    comparePodcastWithCurrent(podcast)
    {
      if(this.currentPodcast)
      {
        return podcast.id === this.currentPodcast.id;
      }      
      return false;
    },
    handlePreviousTrack()
    {
      if(this.currentIndexPodcast > 0)
      {
        this.currentPodcastClips[this.currentIndexPodcast--];
      }        
    },
    handleNextTrack()
    {
      if(this.currentIndexPodcast < this.currentPodcastClips.length)
      {
        this.currentPodcastClips[this.currentIndexPodcast++];
      }
    },
  },
  computed :
  {
    getClip()
    {
      return this.currentPodcastClips[this.currentIndexPodcast];
    }
  },
  components: {
    //FuPlayer,
    // FuHeader,
    //FuPodcast,
    FuNavbar,
    FuMain
  },
  created(){
    fetch(' https://api.audioboom.com/channels/recommended')
      .then(response => response.json())
      .then(data => data.body.filter(d => d.urls.banner_image.original != null))
      .then((podcasts) => {
        this.podcasts = podcasts;
      });
  },  
}

// Developed by : Jose Antonio Alvino Velasque
// 2020
</script>

<style>
body
{
  margin : 0;
}

#app 
{
  font-family: Avenir, Helvetica, Arial, sans-serif;    
  display: flex;
}

.app-player
{
  width: 100%;
  position: fixed;
  bottom: 0;
}

.app-podcasts
{
  display: flex;
  flex-wrap:wrap;
  justify-content: start;
  margin: 85px 0;

}

.app-podcast
{
  margin-bottom:32px;
  margin-left: 16px;
  cursor: pointer;
}


</style>
