  <script>
  export default {
  name: "CardItem",
  props:{
    data:{
      type: Object,
      default: () =>{}
    }
  }
  }
  </script>

<template>
  <div class="card-item">
    <div class="card-item__name">
      {{ data.name }} 
      <span>
        {{ data.lastname }}
      </span>
    </div>
    <div class="card-item__data">
      <div class="">
        {{ data.date}} 
      </div>
      <div>
        
      </div>
    </div>
  </div>
</template>
<style lang="scss">
.card-item{
  width: 100%;
  height: 50px;
  background-color: white;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 16px;
  &__name{
    font-size: 14px;
    line-height: 16px;
    color: black;
    & span{
      font-weight: bold;
    }
  }
  &__data{
    display: flex;
    color: black;
  }
}
</style>