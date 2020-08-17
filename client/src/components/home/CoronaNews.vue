<template>
  <div>
    <v-card  style="box-shadow: none;">
      
        <span v-if="lastUpdate[0]"><b>최근 업데이트:</b> {{ lastUpdate[0].last_build | moment('YYYY.MM.DD HH:mm') }}</span>
       <!-- <v-btn text icon color="green" @click="resetNews()" class="ml-5">
          <i class="material-icons md-dark">update</i>
        </v-btn> -->
      <v-card-title style="padding-top: 0px; padding-bottom: 0px;">
              </v-card-title>
      <v-data-table
        :headers="headers"
        :items="coronaArticles"
        :items-per-page=10
      >
      <template v-slot:item.title={item}>
        <a :href="item.originallink" target="_blank" style="color: black" v-html="item.title"> {{ item.title }} </a>
      </template>
      <template v-slot:item.pubDate={item}>
        {{ item.pubDate | moment('YYYY.MM.DD HH:mm') }}
      </template>
      </v-data-table>
    </v-card>
  </div>
</template>

<script>
export default {
    name: 'CoronaNews',
    props: {
        coronaArticles :{type : Array},
        lastUpdate :{type : Array}
    },
    data(){
      return{
        headers: [
        {
          text: 'Title',
          align: 'left',
          sortable: false,
          value: 'title',
          width: "70%"
        },
        { 
          text: 'Published Date', 
          value: 'pubDate' 
        },
        ],
      }
    },
    methods: {
      resetNews() {
        this.$emit('refresh')
      }
    }
   
}
</script>

<style>

</style>