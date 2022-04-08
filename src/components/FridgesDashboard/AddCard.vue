<template>
  <router-link class="router-link add-card" to="/createfridge">
      Create Fridge
  </router-link>
</template>

<script>
export default {

}
</script>

<style>
.router-link {
  text-decoration: none;
}
.fridge-plus {
  font-size: 1rem;
  fill: black;
}
.add-card {
  /* background: rgb(228, 228, 228); */
  background: #f2f2f2;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 60px;
  border: 1px solid #BDBDBD;
  /* box-shadow: 1px 1px 6px 0 rgba(31, 38, 135, 0.3); */
  font-family: 'Montserrat', sans-serif;
  font-weight: 500;
  color: #403D39;
  padding-top: 8px;
  padding-bottom: 8px;
  transition: 0.3s;
}
.add-card:hover{
  background: #cccccc;
  color: #403D39;
}

</style>