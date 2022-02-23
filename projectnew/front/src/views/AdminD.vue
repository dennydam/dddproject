<template>
  <v-app>
    <v-container mt-15>
      <v-card class="mx-auto card ms-16" height="200" width="256">
        <v-navigation-drawer class="white" dark permanent>
          <v-list>
            <v-btn class="pa-2" block color="primary" dark to="/admin/products">
              商品管理
            </v-btn>
          </v-list>

          <v-list>
            <v-btn class="pa-2" block color="primary" dark to="/admin/orders">
              飲食管理
            </v-btn>
          </v-list>

          <template v-slot:append>
            <div class="pa-2">
              <v-btn block dark to="/"> Home </v-btn>
            </div>
          </template>
        </v-navigation-drawer>
      </v-card>
      <!-- <v-row>
        <v-col cols="12" class="text-center">
          <v-btn rounded color="primary" dark to="/admin/products">
            商品管理
          </v-btn>
          <v-btn rounded color="primary" dark> 訂單管理 </v-btn>
        </v-col>
      </v-row> -->
    </v-container>
    <router-view></router-view>
  </v-app>
</template>
<script>
export default {
  data () {
    return {}
  }
}
</script>
<style>
.card {
  position: absolute;
  left: 20px;
  z-index: 111;
}
</style>
