<template>
<div class="fridge-container">
    <div class="row justify-content-center title-div">
        <div class="col-12 title-col">
            <h4 style="font-weight: 700;">Browse items</h4>
        </div>
    </div>
    <div class="row justify-content-center content-div">
        <router-link class="create-btn" to="/home">Add Item</router-link>
        <ItemCard class="item" v-for="item in items" :key="item.id" :item="item"/>
    </div>
</div>
</template>

<script>
import axios from "axios";
import {eventBus} from "../../main";
import ItemCard from "./ItemCard.vue";

export default {
    name: "ListFridges",
    components: {
        ItemCard
    },
    data() {
        return {
            items: [],
        };
    },
    created() {
        this.getItems();
        eventBus.$on(["create-fridge-success", "delete-fridge-success"], () => {
            this.getFridges();
        });
    },
    methods: {
        getItems() {
            axios
                .get("/api/item/all")
                .then((response) => {
                    this.items = response.data;
                })
                .catch((error) => {
                    alert(error);
                });
        },
    },
};
</script>

<style scoped>
.fridge-container {
    color: #403D39;
    font-family: "Montserrat", sans-serif;
    padding: 0 30px;
    width: 90%;
}
/* .title-div{
    margin: 0 2vw;
    border-bottom: 2px solid black;
} */
h4{
    margin-bottom:0;
}
.content-div{
   margin-top: 1vh;
}
.col-12 {
    padding: 0;
}
.title-col{
    font-size: 2rem;
}
.empty-placeholder {
    margin: 4vh 2vw;
}
.items-grid {
    display: flex;
    flex-wrap: wrap;
    padding: 0;
    margin-top: 1vh;
}
.create-btn{
    text-decoration: none;
    width: 100%;
    text-align: center;
    background: white;
    border-radius: 99px;
    border: 1px solid #BDBDBD;
    font-weight: 500;
    color: #403D39;
    padding-top: 8px;
    padding-bottom: 8px;
    margin-bottom: 10px;
    transition: 0.3s;
}
.create-btn:hover{
    background: #cdff61;
    border: 1px solid #cdff61;
    color: #403D39;
}
</style>
