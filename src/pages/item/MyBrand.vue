<template>
  <v-card>
    <v-card-title>
      <v-btn color="primary" @click="addBrand">新增品牌</v-btn>
      <v-spacer />
      <v-text-field label="输入关键字搜索" v-model="search" append-icon="search" hide-details/>
    </v-card-title>
    <v-data-table
      :headers="headers"
      :items="brands"
      :pagination.sync="pagination"
      :total-items="totalBrands"
      :loading="loading"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-center">{{ props.item.id }}</td>
        <td class="text-xs-center">{{ props.item.name }}</td>
        <td class="text-xs-center"><img v-if="props.item.image" :src="props.item.image" width="130" height="40"/></td>
        <td class="text-xs-center">{{ props.item.letter }}</td>
        <td class="text-xs-center">
          <v-icon small class="mr-2" @click="editItem(props.item)">
            edit
          </v-icon>
          <v-icon small @click="deleteItem(props.item)">
            delete
          </v-icon>
        </td>
      </template>
    </v-data-table>
    <v-dialog max-width="500" v-model="show" persistent>
      <v-card>
        <v-toolbar dense dark color="primary">
          <v-toolbar-title>新增品牌</v-toolbar-title>
          <v-spacer/>
          <v-btn icon @click="closeWindow">
            <v-icon>close</v-icon>
          </v-btn>
        </v-toolbar>
        <v-card-text class="px-5">
          <my-brand-from fromValue="name"/>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
  import MyBrandFrom from './MyBrandFrom'
  export default {
    name: "myBrand",
    components: {MyBrandFrom},
    data () {
      return {
        name: "111111111",
        totalBrands: 0, // 总条数
        brands: [], // 当前页品牌数据
        loading: true, // 是否在加载中
        pagination: {}, // 分页信息
        search: "", //搜索关键字
        headers: [ // 头信息
          {text: 'id', align: 'center', value: 'id'},
          {text: '名称', align: 'center', value: 'name'},
          {text: 'LOGO', align: 'center', value: 'image', sortable: false},
          {text: '首字母', align: 'center', value: 'letter'},
          {text: '操作', align: 'center', value: 'id', sortable: false }
        ],
        show: false
      }
    },
    watch:{
      pagination:{
        deep: true,
        handler(){
          this.getDataFromServer();
        }
      },
      search(){
        this.pagination.page = 1;
        this.getDataFromServer();
      }
    },
    methods: {
      getDataFromServer(){ // 从服务端加载数据的函数
        this.loading = true;
        const param = {
          name: this.search,
          pageSize: this.pagination.rowsPerPage,
          pageNum: this.pagination.page,
          sortBy: this.pagination.sortBy,
          desc: this.pagination.descending
        }
        this.$http.get("item/brand/page",{
          params:{
            params: JSON.stringify(param)
          }
        }).then(resp => {
          console.log(resp.data);
          this.totalBrands = resp.data.totalSize; // 总条数
          this.brands = resp.data.data; // 品牌数据
          this.loading = false; // 加载完成
        });
      },
      addBrand(){
        this.show = true;
      },
      closeWindow(){
        this.show = false;
      }
    },
    // 渲染后执行
    mounted(){
      this.getDataFromServer() // 调用数据初始化函数
    }
  }
</script>

<!-- scoped:当前样式只作用于当前组件的节点 -->
<style scoped>

</style>
