<template>
  <div  class=" max-w-screen-2xl  mx-auto  p-4 rounded-lg">
    <div class="flex flex-col">
      <div class="w-full flex justify-center">
        <img class="rounded-full border-8 max-h-60 mb-4 border-zinc-800" :src="list.image" alt="">
      </div>
      <div>
        <div class="flex w-full mb-4 text-4xl text-center text-black justify-center font-bold">
          <h2>{{list.name}}</h2>
        </div>
      </div>
    </div>
    <div class="flex flex-wrap bg-zinc-800 rounded-lg">
      <div class="w-full">
        <h4 class="text-white text-3xl py-2 mt-4 font-extrabold">Episode</h4>
      </div>
      <CardView v-for="item in episode" page="episode" :status="list.status" :title="item.name" :species="item.air_date"  :key="item.index" />
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  import CardView from "../components/CardView.vue";
  import {useRouter} from "vue-router";
  //can use only in setup()
  export  default {
    name : 'CharacterDetailView',
    data(){
      return {
        list : [],
        episode: null,
        id : null,
      }
    },
    components: {CardView},
    methods: {
      getData : function (event){
        axios.get('https://rickandmortyapi.com/api/character/'+event).then(
                response => this.list = response.data,
        );
      }
    },
    watch : {
      id: function (){
        this.getData(this.id);
      },
      list : function (){
        this.episode = [];
        this.list.episode.forEach(element =>
                axios.get(element).then(
                        response => this.episode.push(response.data),
                )
          )
      },
    },
    created() {
      this.id = useRouter().currentRoute.value.query.id;
    }
  }
</script>