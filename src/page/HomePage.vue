<template>
    <div class="home--page">
        <div class="header">
            <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg" alt="" srcset="">
            <input type="text" placeholder="De quoi avez-vous envie ?">
        </div>
        <div class="bannier"></div>
        <!-- Passer via props les data, passer des données d'un component parent à un component enfant -->
        <Restaurant-Row v-for="(data, i) in data_restaurant" :key="i" :three_restaurant="data"/>
        <!-- <Restaurant-Row/> -->
    </div>
</template>

<script>
// IMPORT
import BDD from '../BDD.js'
import { onMounted, ref } from 'vue'
// COMPONENTS
import RestaurantRow from '../components/RestaurantRow.vue';

export default {
    name: 'HomePage',
    components: {
        RestaurantRow,
    },
    setup() {
        class Restaurant {
            contructor (name, note, image, drive_time) {
                this.name = name
                this.note = note
                this.image = image
                this.drive_time = drive_time
            }
        }

        // Réf = variable observable permet de récup la data et d'être exécuter avant
        let data_restaurant = ref([]);

        const makeDataRestaurant = () => {
            
            let three_restaurant = [];

            for (const restaurant of BDD) {
                // console.log(restaurant);
                const new_Restaurant = new Restaurant(restaurant.name, restaurant.note, restaurant.image, restaurant.drive_time);

                if (three_restaurant.length === 2) {
                    three_restaurant.push(new_Restaurant);
                    data_restaurant.value.push(three_restaurant);
                    three_restaurant = [];
                } else {
                    three_restaurant.push(new_Restaurant);
                }
            }
            // console.log(data_restaurant);
        }
        // Permet de charger la fonction quand tout les composants sont créés (montés)
        // makeDataRestaurant();
        onMounted(makeDataRestaurant);

        return {
            data_restaurant,
        }
    },
}
</script>

<style lang="scss">
.home {
    .header {
        height: 120px;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        
        img {
            width: 200px;
        }

        input {
            background-color: #f6f6f6;
            border: none;
            height: 60px;
            width: 400px;
            outline: none;
            padding-left: 20px;
        }
    }
    .bannier {
        height: 200px;
        width: 100%;
        // background-image:url("");
        background-color: black;
        background-size: cover;
        background-position: center center;
    }
}
</style>