<template>
  <div id="app">
    <fu-navbar
     :current_menu_selected="menuSelected"
     @handle_menu_selected="handleMenuSelected"
    />
    <fu-main
     :current_menu_selected="menuSelected"
     :clips="clips"
     :current_clip_id="currentPlayList.id"
     @handle-click-tracks="handleClickTracks"
     @get-search="getSearch"
    />
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
    <div 
     class="app-player"
     v-if="currentPlayListClips.length > 0"
    >
      <fu-player 
       :name="getClip.title_short"
       :url="getClip.preview"
       :image_url="getCover"
       :album="getTitle"       
       :current_clip_index="currentIndexClip"
       :playlist_length="currentPlayListClips.length"
       @next-track="handleNextTrack"
       @previous-track="handlePreviousTrack"
       @handle-play="handlePlay"
      />
    </div>
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
      clips : [],
      currentPlayList : {id : 0},
      currentPlayListClips :[],
      currentIndexClip : 0,
      menuSelected : 'playListTracks',  
      cover : '',
      isPlaying: false,
    }
  },
  methods:
  {
    handleMenuSelected(menuSelected)
    {
      if(!this.isPlaying)
      {
        this.clips = [];
        this.currentPlayList = {id : 0};
        //this.currentPlayListClips = [];
        this.currentIndexClip = 0;
      }      
      this.menuSelected = menuSelected;
      this.getInit();
    },
    async handleClickTracks(item, type)
    {
      //console.log(item, type);
      const {id} = item;
      this.currentPlayList = item;
      //console.log(this.currentPlayList.title);
      const result = await this.getClips(id, type);
     // console.log(result);
      this.currentPlayListClips = result;
      this.currentIndexClip = 0;

    },
    getClips(id, type)
    {
      console.log(id, type);
      let listClips;
      switch(type)
      {
        case "playListTracks":
           listClips = fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/playlist/${id}`)
                            .then((response) => response.json())
                            .then((data) => {
                              if(data.tracks)
                              {
                                 return data.tracks.data;
                              } else
                              {
                                return []
                              }
                            });
          break;
        case "albumTracks":
           listClips = fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/album/${id}`)
                            .then((response) => response.json())                            
                            .then((data) => {
                              console.log(data);
                              if(data.tracks)
                              {
                                this.cover = data.cover_medium;
                                return data.tracks.data;
                              } else
                              {
                                return []
                              }
                              
                            });
          break;
        case "artistTracks":
           listClips = fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/artist/${id}/top?limit=50`)
                            .then((response) => response.json())
                            .then((data) => data.data)
                            // .then((data) => console.log(data));
          break;
        default :
          listClips = null;
          break;        
      }
      return listClips;
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
      if(this.currentIndexClip > 0)
      {
        this.currentPlayListClips[this.currentIndexClip--];
      }        
    },
    handleNextTrack()
    {
      if(this.currentIndexClip < this.currentPlayListClips.length)
      {
        this.currentPlayListClips[this.currentIndexClip++];
      }
    },
    getInit()
    {
      switch(this.menuSelected)
      {
        case 'artistTracks':
          fetch('https://cors-anywhere.herokuapp.com/https://api.deezer.com/chart/top?limit=50')
              .then(response => response.json())      
              .then((data) => {
                this.clips = data.artists.data;
                //console.log("artist",data.artists.data);
              });
          break;
        case 'albumTracks':
          fetch('https://cors-anywhere.herokuapp.com/https://api.deezer.com/chart/top?limit=50')
              .then(response => response.json())      
              .then((data) => {
                this.clips = data.albums.data;
                //console.log("album",data.albums.data);
              });
          break;
        default :
          fetch('https://cors-anywhere.herokuapp.com/https://api.deezer.com/chart/top?limit=50')
              .then(response => response.json())      
              .then((clips) => {
                this.clips = clips.playlists.data;
                // console.log(clips.playlists.data);
              });
          break;
      }
    },
    getSearch(search, type)
    {
      switch(type)
      {
        case "playListTracks":          
            fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/search?q=track:${search}`)
                  .then(response => response.json())      
                  .then((data) => {
                    this.clips = data.data;
                    //console.log("Tracks : ", data);
                  });
          break;
        default:
            fetch(`https://cors-anywhere.herokuapp.com/https://api.deezer.com/search?q=album:${search}`)
                  .then(response => response.json())      
                  .then((data) => {                    
                    this.clips = data.data;
                    //console.log(type,data);
                  });       
          break;
      }      
    },
    handlePlay()
    {
      this.isPlaying != this.isPlaying;
    },
  },
  computed :
  {
    getClip()
    {
      return this.currentPlayListClips[this.currentIndexClip];
    },
    getCover()
    {      
      if(this.getClip.album)
      {
        return this.getClip.album.cover_medium
      } else if(this.currentPlayList.cover_medium)
      {
        return this.currentPlayList.cover_medium;
      } else
      {
        return this.cover;
      } 
    },
    getTitle()
    {
      if(this.getClip.album)
      {
        return this.getClip.album.title;
      } else
      {
        return this.getClip.title;
      }  
    },    
  },
  components: {
    FuPlayer,
    FuNavbar,
    FuMain
  },
  created(){
        this.getInit();
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
