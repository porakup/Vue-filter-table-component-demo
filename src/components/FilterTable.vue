<template>
<div>
    <v-form>
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="4">
          <v-text-field
            v-model="filter"
            label="Filter"
            @input="filterData"
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
    <v-data-table 
    :disable-pagination="true"
    :hide-default-footer="true"
    :headers="column"
    :items="dataList"
    class="elevation-1">
    </v-data-table>
</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'FilterTable',
  data() {
    return {
      masterList:[],
      dataList:[],
      filter:'',
      column:[
             { text: 'No.',value:'no'},
             { text: 'Name',value:'name'},
      ]
    }
  },
   mounted: function() {
    this.$nextTick(async ()=>{
      this.masterList = await axios.get('https://api.publicapis.org/categories')
      .then(resp => resp.data.map((data,index)=> {
          return {no:index+1,name:data}
      }));
      this.dataList = this.masterList
    })
  },
  methods:{
      filterData(){
          if(this.dataList.length && this.filter.length){
              let filterList = [];
              for(let i = 0; i < this.dataList.length; i++){
                  let obj = this.dataList[i]
                  if(obj.name.toUpperCase().indexOf(this.filter.toUpperCase()) > -1){
                      filterList.push(obj)
                  }

              }
              this.dataList = filterList;

          } else {

          this.dataList = this.masterList

          }
      }
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
