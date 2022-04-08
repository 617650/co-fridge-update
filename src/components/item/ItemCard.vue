<template>
  <div class="row item">
    <button class="delete-button" 
      v-show="signedInUser===item.creatorID"
      v-on:click="DeleteItem">X</button>
    <br>
    <div class="row info" style="margin:0; padding: 0">
      <div style="margin:0; padding: 0">
        <img src="https://upload.wikimedia.org/wikipedia/commons/a/ad/Circle_%28transparent%29.png" class="user-picture">
        <span style="padding: 10px"><span class="text-bold">{{item.creatorID}}</span> in {{item.fridge[0]}}</span>
      </div>
    </div>
    <div class="row info-row">
      <div class="col-2" style="margin-right: 30px;">
        <div class="label-top">{{item.category}}</div>
        <div class="large">{{item.name}}</div>
      </div>
      <div class="col-2" style="margin-right: 30px;">
        <div style="margin: 5px"><span class="content">{{item.quantity}}</span> Count</div>
        <div style="margin: 5px"><span class="content">{{item.expiryDate}}</span> until expire</div>
        <div style="margin: 5px"><span class="content">{{item.distance}}</span> miles from you</div>
      </div>
      <div class="col-2" style="margin-right: 30px;">
        <div class="label">Pick-up</div>
        <div class="content">{{item.date}}</div>
        <div class="content">{{item.start}} - {{item.end}}</div>
      </div>
      <div class="col-2" style="margin-right: 30px;">
        <div class="label">Fridges</div>
        <div v-for="fridge in item.fridge" :key="fridge" class="content">{{fridge}}</div>
      </div>
      <div class="col-2" style="text-align: center">
        <img
          v-if="item.imageUrl"
          :src="item.imageUrl"
          contain
          width="100%"
        />
      </div>
    </div>
    
    <!-- <div class="col-2">
        <div v-show="item.creatorID!=signedInUser"> 
        <ClaimButton :item="item" />
        </div>
    </div> -->

  </div>
</template>

<script>
import { eventBus } from "../../main";
import axios from "axios";
//import ClaimButton from "@/components/item/ClaimButton.vue";
export default {
  name: "Item",
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  components: {  },
  data() {
    return {
      signedInUser: this.$cookie.get("fritter-auth"),
      quantity: null,
      status: "",
      user: "",
      message: "",
    };
  },
 
  mounted: function () {
    this.getStatus();
    eventBus.$on("claim-item-success", () => {
      this.getStatus();
    });
  },
  //&& item.creatorID != signedInUser
  methods: {
    DeleteItem() {
      axios
        .delete("/api/item/delete/" + this.item.id)
        .then((response) => {
          eventBus.$emit("delete-item-success", {
            data: response.data,
          });
        })
        .catch((error) => {
          if (error.response && error.response.status != 200) {
            alert(error.response.data.error);
          }
        });
    },
    getStatus() {
      if (this.item.status === 0) {
        this.status = "Available";
      } else {
        this.status = "On Hold";
      }
    },
    // getUser(){
    //   axios.get("api/user/"+this.signedInUser)
    //   .then((response) => {
    //     this.user = response.data.
    //   })
    // }

    ModifyItem() {},
    SubmitModifiedItem() {
      //   axios
      //     .put("/api/freets/edit/" + this.freet.id, this.NewFreet)
      //     .then((response) => {
      //       eventBus.$emit("edit-freet-success", {
      //         data: response.data,
      //       });
      //       this.open = false;
      //     })
      //     .catch((error) => {
      //       if (error.response && error.response.status != 200) {
      //         this.error = error.response.data.error;
      //       }
      //     });
      // },
    },
  },
};
</script>

<style scoped>
.row{
  padding: 0;
  margin: 0;
}
.item {
  width: 100%;
  padding-top: 10px;
  margin: 15px;
  margin-bottom: 18px;
  border-top: 1px solid #BDBDBD;
  font-size: 0.9rem;
}
.item > p {
  overflow-wrap: break-word;
  font-size: 11pt;
}
.item > h1 > span {
  font-weight: 100;
  display: flex;
  font-size: 20pt;
  color: #104763;
}
.info{
  padding: 0;
  font-size: 0.8rem;
}
.info-row{
  padding: 20px 0 10px 40px;
  font-size: 0.8rem;
}


.author {
  display: flex;
  align-items: center;
  font-size: 11pt;
  font-weight: bold;
}

.author > span {
  margin-right: 20px;
}

.delete-button {
  float: right;
  border-radius: 5px;
  box-shadow: 1px 1px 6px 0 rgba(31, 38, 135, 0.3);
  
}
.edit-button {
  float: right;
}
.text-bold{
  font-weight: 600;
}

.user-picture {
    background: white;
    border: 1px solid #cdff61;
    height: 40px;
    width: 40px;
    border-radius: 100%;
}
.label{
  margin-bottom: 15px;
}
.large{
  font-size: 1.2rem;
  font-weight: 600;
}
.content{
  font-weight: 600;
}
</style>
