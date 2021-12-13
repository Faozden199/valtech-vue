<template>
  <div class="main">
    <div class="card-container">
        <Card v-for="city in cities" :key="city.city" orientation="horizontal" :city="city"  />
    </div>
  </div>
</template>

<script>
import config from '../../config';
import axios from 'axios';
import Card from './Card'

export default {
  components: { Card },
    name: 'Main',
    props: {
        msg: String
    }, 
    data: () => {
        return{
            cities: [
             { 
                country: 'front-end',
                name: 'valtech_',
                tags: 'valtech',
                size: 'size-4',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'copenhague.jpg',
                flickr: false
            },
            { 
                country: 'Italy',
                name: 'Venice',
                tags: 'Venice',
                size: 'size-2',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'venise.jpg',
                flickr: false
            },
            { 
                country: 'Germany',
                name: 'Berlin',
                tags: 'Berlin',
                size: 'size-1',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'berlin.jpg',
                flickr: false
            },
            { 
                country: 'Spain',
                name: 'Barcelona',
                tags: 'Barcelona',
                size: 'size-1',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'barcelone.jpg',
                flickr: false
            },
            { 
                country: 'France',
                name: 'Paris',
                tags: 'Paris',
                size: 'size-1',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: '',
                flickr: false
            },
            { 
                country: 'Netherlands',
                name: 'Amsterdam',
                tags: 'Amsterdam',
                size: 'size-1',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'manchester.jpg'
            },
            { 
                country: 'United Kingdom',
                name: 'London',
                tags: 'London',
                size: 'size-2',
                description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua',
                image_link: 'londre.jpg',
                flickr: false
            }
            ],
            loading: true
        }
    },
    mounted() {
        this.cities.forEach(city => {
            if(!city.image_link){
                console.log("Pas de lien renseigné, fetching launched")
                this.fetchImages(city.tags)
                    .then((response) => {
                        city.image_link = response.data.photos.photo[0].url_n
                        city.flickr = true;
                    }).catch(error => {
                    this.errorMessage = error;
                    console.error("Network error", error);
                    city.image_link = 'copenhague.jpg'
                    });
            }
        });
        this.loading = false;
    },
    methods: {
        // Si nous n'avons pas d'image de disponible, 
        // on peut utiliser la library Fetch en cherchant via les tags.
        fetchImages(city_tags) {
            return axios({
                method: 'get',
                url: 'https://api.flickr.com/services/rest',
                params: {
                method: 'flickr.photos.search',
                api_key: config.api_key,
                tags: city_tags,
                extras: 'url_n, owner_name, date_taken, views',
                page: 1,
                format: 'json',
                nojsoncallback: 1,
                per_page: 1,
                }
            })
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.main{
    max-width: 1300px;
    margin: 0 auto;
}

.card-container{
    display: flex;
    flex-wrap: wrap;
    list-style: none;
    margin: 0;
    padding: 0;
    justify-content: center;
}
</style>

