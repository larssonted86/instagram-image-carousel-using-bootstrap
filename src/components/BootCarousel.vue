<template>
<div class="container">
  <div class="row mb-2 p-1">
    <!-- Total images selection section -->
    <div class="col d-flex  justify-content-end">
        <label for="total_amount" class="text-white">totalt antal bilder</label>
        <select @change="image_amount = ChangeAmount()" name="total_amount" id="total_amoutn" v-model="total_amount" > 
            <option value="12">12</option>                    
            <option value="24">24</option> 
            <option value=48>48</option> 
            <option value="96">96</option>
            <option :value="data.length">alla</option> 
        </select>
    </div>
  </div>
  <div class="row">
    <div class="col" :key="force_update">
      <div id="carouselExampleIndicators" class="carousel slide carousel-container m-3" data-bs-ride="carousel" data-bs-interval="false">
        <div class="carousel-indicators">
          <button v-for="(set,index) in set_of_images" :key="index"  type="button" data-bs-target="#carouselExampleIndicators" :data-bs-slide-to="index" :class="`${ index === 0 ? 'active' : '' }`" aria-current="true" :aria-label="'Side '+index"></button>
        </div>
        <div class="carousel-inner">
          <div v-for="(set,index) in set_of_images" :key="index" :class="`carousel-item ${ index === 0 ? 'active' : '' }`">
            <div class="slide-container">
              <div class="row slider-row">
                <div 
                  v-for="image in set" 
                  :key="image.id" 
                  class="col-6  col-md-4 col-lg-3 image-col position-relative mb-2 d-flex justify-content-center"  
                  @mouseenter="image.showCaption = true"
                  @mouseleave="image.showCaption = false"
                  :style="{ 
                    backgroundImage: image.thumbnail_url ? `url('${image.thumbnail_url}')`:  `url('${image.media_url}')`,
                  }" >
                    <a  :href="image.permalink" class="caption-link">
                      
                          <div v-show="image.showCaption" class="position-absolute bottom-0 bg-dark caption-container">
                              {{image.caption}}
                          </div>                        
                     
                    </a>
                </div>
              </div>
            </div>
          </div>
        </div>
        <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="prev">
          <span class="carousel-control-prev-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Previous</span>
        </button>
        <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleIndicators" data-bs-slide="next">
          <span class="carousel-control-next-icon" aria-hidden="true"></span>
          <span class="visually-hidden">Next</span>
        </button>
      </div>
    </div>
  </div>
</div>
   
  
</template>

<script>
    import axios from 'axios';

export default {
    name: "bootcarousel",
    data(){
        return{
            data: [],
            total_amount: localStorage.total_amount ?? 12,
            number_per_slide: localStorage.number_per_slide ??  6,
            set_of_images:[],
            token: process.env.VUE_APP_INSTAGRAM_TOKEN,
            force_update: 0,
        }

    },

    methods:{

        //fetches the data from instagram and adds a boolean value to each object to help with display of captions
        get_data(){
             axios.get('https://graph.instagram.com/me/media?fields=id,caption,media_url,thumbnail_url,permalink&access_token='+this.token + '&limit='+this.total_amount)
                    .then(res=>{
                        this.data = res.data.data;
                        for (let element of this.data) {
                             element.showCaption = false 
                        }  
                            return this.data
                    }).then(data =>{
                      this.set_of_images = this.chunkArray(data, this.number_per_slide)
                      console.log(this.set_of_images)
                      this.force_update++
                    } );
        },

        chunkArray(array, size) {
            if(array.length <= size){
            return [array]
          }
          return [array.slice(0,size), ...this.chunkArray(array.slice(size), size)]
        },

        //changes the amount of total pictures in the carousel
        ChangeAmount(){

          localStorage.total_amount = this.total_amount;

          this.get_data();
    },
    },
    mounted(){
       this.get_data();
    },
}
</script>

<style lang="scss" scoped>


.carousel-container{
  height: 300px
}

.carousel-inner{
    height:100%;
}
.carousel-item{
  height:100%;
}
.slide-container{
  height: 100%;
}
.slider-row{
  height:100%;
}
.image-col{
  background-size: contain;
  background-repeat: no-repeat;
  background-position: center;
}
.caption-container{
    width:100%;
    padding: 5px;
    height: 30px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
.caption-link{
    color: white;
    text-decoration: none;
    font-size: 12px;
    &:hover{
        color: white;
    }
}
</style>