<template>
  <div class="fridge-card">
    <router-link :to="{name: 'fridge', params: {fridge: fridge, name: fridge.name}}" class="f-subcard"  >
        <div class="card-info card-title">{{fridge.name}}</div>
        <div class="card-info sub-info">02138</div>
        <div class="card-info sub-info">{{ fridge.items.length }} items | {{ fridge.members.length }} members</div>
    </router-link>
    <!-- <button v-on:click="DeleteFridge" class="del-fridge-btn" >
            Delete
    </button> -->
  </div>

</template>

<script scoped>
import { eventBus } from "../../main";
import axios from "axios";

export default {
    name: "FridgeCard",
    props: {
        fridge: {
            type: Object,
            required: true,
        }
    },
    data() {
        return{}
    },
    methods:{
        DeleteFridge(){
            axios.delete("/api/Fridges/" + this.fridge.fridgeID)
                .then((response) => {
                    eventBus.$emit("delete-fridge-success",{
                        data: response.data,
                    });
                })
                .catch((error) => {
                    if (error.response && error.response.status != 200){
                    alert(error.response.data.error)
                    }
                })
            
        },
    }
}
</script>

<style>
.fridge-card {
    width: 100%;
    margin-top: 1vh;
}
.f-subcard {
    width: 100%;
    height: 90px;
    display: flex;
    flex-wrap: wrap;
    justify-content: left;
    align-items: center;
    padding: 10px 20px;
    text-decoration: none;
    color: #403D39;
    background-color: white;
    border-radius: 10px;
    border: 1px solid #BDBDBD;
    font-weight: 500;
    font-size: 0.85rem;
    transition: 0.3s;
    /* box-shadow: 1px 1px 6px 0 rgba(31, 38, 135, 0.3); */
}
.f-subcard:hover{
    background: #f2f2f2;
}
.del-fridge-btn{
    margin-top: -60px;
    background: #f6652c;
    color: white;
    border: none;
    border-radius: 5px;
    box-shadow: 1px 1px 6px 0 rgba(31, 38, 135, 0.3);
}
.card-info{
    width: 100%;
    margin: 0;
}
.card-title{
    font-weight: 700;
    margin: 0;
}
.sub-info{
    color: #999999;
}
</style>