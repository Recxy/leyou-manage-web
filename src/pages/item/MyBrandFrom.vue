<template>
    <v-form v-model="valid">
      <v-text-field v-model="brand.name" label="请输入品牌名称" required/>
      <v-text-field v-model="brand.letter" label="请输入品牌首字母" required/>
      <v-cascader
        url="/item/category/list"
        multiple
        required
        v-model="brand.categories"
        label="请选择商品分类"/>
    </v-form>
  <v-layout row>
    <v-flex xs3>
      <span style="font-size: 16px; color: #444">品牌LOGO：</span>
    </v-flex>
    <v-flex>
      <v-upload
        v-model="brand.image"
        url="/upload"
        :multiple="false"
        :pic-width="250"
        :pic-height="90"
      />
    </v-flex>
  </v-layout>
</template>

<script>
    export default {
      name: "my-brand-from",
      data(){
        return {
          valid: false,
          brand: {
            name:'', // 品牌名称
            letter:'', // 品牌首字母
            image:'',// 品牌logo
            categories:[], // 品牌所属的商品分类数组
          }
        }
      },
      props:{
        fromValue: String
      }
    }
</script>

<style scoped>

</style>
