<template>
  <div class="charter">
    <h1>{{textAbout}}</h1>
    <div class="search 	my-2 mx-auto">
      <input type="text" v-model="search" placeholder="Search" class="border border-current	rounded-sm border-solid px-2 py-1">
    </div>
    <div class="list">
      <CardView page="character" v-for="item in filterSearch" :key="item.id" :id="item.id" :images="item.image" :title="item.name" :status="item.status" :species="item.species" />
    </div>
    <PaginationHome @item="item=$event" :pages="pagition" :item="item" :firstpage="[1,2,3,4,5]"></PaginationHome>
  </div>
</template>
<script>
  import axios from "axios";
  import PaginationHome from "../components/PaginationHome.vue";
  import CardView  from "../components/CardView.vue";

  export default {
    name: 'CharterView',
    data(){
      return{
        textAbout : 'This is an Character page',
        item : 1,
        list : null,
        pagination : null,
        search: '',
      }
    },
    mounted(){},
    methods:{
      getData : function (event){
        axios.get('https://rickandmortyapi.com/api/character?page='+event).then(response => this.list = response.data.results);
      },
      listPage : function (event) {
        axios.get('https://rickandmortyapi.com/api/character?page='+event).then(response => this.pagition = response.data.info.pages);
      },
    },
    computed: {
      filterSearch : function(){
        if (this.search){
          return this.list.filter(lists=> {
            return  lists.name.toLowerCase().includes(this.search.toLowerCase());
          })
        }
        else{
          return this.list;
        }
      }
    },
    watch : {
      item : function (){
        this.getData(this.item);
      }
    },
    components: {
      PaginationHome,
      CardView
    },created() {
      this.getData(this.item);
      this.listPage();
    },
  }
</script>
<style scoped>
  .charter{
    max-width: 1200px;
    padding: 0 15px;
    margin: 0 auto;
  }
  .list{
    display: flex;
    flex-wrap: wrap;
    margin: 0 -15px;
  }
  .item{
    width: calc(25% - 30px);
    margin: 0 15px 30px;
  }
  .item img{
    width: 100%;
  }
  .Alive{
    background: #24de00;
    color: #101f00;
  }
  .unknown{
    background: #ff7800;
    color: #fff;
  }
  .Dead{
    background: #ff000a;
    color: #fff;
  }
</style>