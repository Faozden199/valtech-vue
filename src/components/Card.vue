<template>
  <div class="card" :class="city.size" @mouseover="mouseover" @mouseleave="mouseleave">
    <div class="card_content" :class="hovering && (city.size !== 'size-4') ? 'card_content_hover' : ''" :style="backgroundStyles">
      <p style="text-transform: uppercase; margin: 0;">{{city.country}}</p>
      <h2 style="text-transform: uppercase; margin-top: 0;">{{city.name}}</h2>
      <p class="description" style="transition: ease-in-out 2s;" v-if="(city.size === 'size-4')">{{city.description}}</p> 
        <transition name="slide-fade">
          <div v-if="hovering && (city.size !== 'size-4')">
            <p class="description" style="transition: ease-in-out 2s;">{{city.description}}</p> 
            <button class="btn">
              Explore More
            </button>
          </div>
        </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: {
    city: Object,
    orientation: String
  },
  data: () => {
    return{
      hovering: false
    }
  },
  computed: {
    backgroundStyles: function(){
      if(this.city.image_link && !this.city.flickr){
        return{
          '--back-image' : 'url(' + require('@/assets/' + this.city.image_link) + ')',
          '--background_hover' : 'linear-gradient(-45deg, rgba(65, 65, 65, 0.5),rgba(65, 65, 65, 0.5)), url(' + require('@/assets/' + this.city.image_link) + ')'
        }
      }
      // Si pas d'image, on met une de base ! 
      return{
        '--back-image' : 'url(' + require('@/assets/copenhague.jpg') + ')',
        '--background_hover' : 'linear-gradient(-45deg, rgba(65, 65, 65, 0.5),rgba(65, 65, 65, 0.5)), url(' + require('@/assets/copenhague.jpg') + ')'
      }
    }
  },

  methods: {
    mouseover: function(){
      this.hovering = true
    },    
    mouseleave: function(){
      this.hovering = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.btn {
  color: #ffffff;
  padding: 0.8rem;
  font-size: 14px;
  border-radius: 4px;
  font-weight: 400;
  min-width: 100px;
  width: 25%;
  cursor: pointer;
  border: 1px solid rgba(255, 255, 255, 1);
  background: transparent;
}

.btn:hover {
  background-color: rgba(255, 255, 255, 0.12);
}

@media (min-width: 0px) {
  .size-4{
    width: 100%;
  }
  .size-2{
    width: 100%;
  }
  .size-1{
    width: 100%;
  }
}

@media (min-width: 375px) {
  .size-4{
    width: 100%;
  }
  .size-2{
    width: 100%;
  }
  .size-1{
    width: 50%;
  }
}

@media (min-width: 768px) {
  .size-4{
    width: 100%;
  }
  .size-2{
    width: 50%;
  }
  .size-1{
    width: 25%;
  }
}

.size-4{
  height: 700px;
}

.card {
  height: 500px;
  padding: 2.5px;
  box-sizing: border-box;
  transition: ease-in-out 2s;
  margin: 2.5px 0px
}

.card_content {
  background: var(--back-image);
  background-size: cover;
  background-position: center;
  height: 100%;
  border-radius: 0.25rem;
  box-shadow: 0 20px 40px -14px rgba(0, 0, 0, 0.25);
  overflow: scroll hidden;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0 2rem;
}

.card_content_hover {
  background: var(--background_hover);
  background-size: cover;
  background-position: center;
}

/* Transition CSS*/
/** Could be implemented latter for now just div apparition*/

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter-from, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}

</style>
