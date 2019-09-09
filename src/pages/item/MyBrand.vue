<template>
  <div>
    <v-layout class="px-4 pb-3">
      <v-flex xs2>
        <v-btn color="success"> 新增</v-btn>
      </v-flex>
      <v-spacer/>
      <v-flex xs4>
        <v-text-field
          label="搜索"
        hide-details append-icon="search" v-model="key"/>
      </v-flex>
    </v-layout>

    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
    <template slot="items" slot-scope="props">
      <td>{{props.item.id}}</td>
      <td class="text-xs-center">{{props.item.name}}</td>
      <td class="text-xs-center"><img :src="props.item.image"></td>
      <td class="text-xs-center">{{props.item.letter}}</td>
      <td class="text-xs-center">
        <v-btn text icon color="blue">
        <v-icon>edit</v-icon>
      </v-btn>
        <v-btn text icon color="error">
          <v-icon>delete</v-icon>
        </v-btn>
      </td>
    </template>
    </v-data-table>
  </div>
</template>

<script>
    export default {
        name: "MyBrand",
        data(){
          return{
            headers:[
              {  text: '品牌id',align: 'center', sortable: true, value: 'id',},
              {  text: '品牌名称',align: 'center', sortable: false, value: 'name',},
              {  text: '图片',align: 'center', sortable: false, value: 'image',},
              {  text: '首字母',align: 'center', sortable: true, value: 'letter',},
              {  text: '操作',align: 'center', sortable: false},
            ],
           // brands:[],
            pagination:{},
            totalBrands:0,
            loading:false,
            key:"",
          }
      },
      created(){


          this.brands=[
            { id:1,name:"小米",image:"1.jpg",letter:"X"},
            { id:2,name:"华为",image:"2.jpg",letter:"H"},
            { id:3,name:"联想",image:"3.jpg",letter:"L"},
            { id:4,name:"魅族",image:"4.jpg",letter:"M"},
          ];
           this.totalBrands=15;
        this.loadBrands();
      },
      watch:{
        key(){
          this.pagination.page=1;
          this.loadBrands();

        },
        pagination:{
          deep:true,
          handler(){
            this.loadBrands();

          }
        }

      },
      methods:{
          loadBrands(){
            this.loading = true;
            this.$http.get("item/brand/page",{
              params:{
                key:this.key,
                page:this.pagination.page,
                rows:this.pagination.rowsPerPage,
                sortBy:this.pagination.sortBy,
                desc:this.pagination.descending,

              }
            }).then(result =>{
              console.log(result);
              this.brands = result.data.result;
              this.totalBrands = result.data.total;
              this.loading = false;
            });
          }
      }
    }
</script>

<style scoped>

</style>
