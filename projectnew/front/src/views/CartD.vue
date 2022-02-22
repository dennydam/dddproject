
<template>
  <v-container>
    <v-simple-table>
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">Name</th>
            <th class="text-left">Calories</th>
            <th class="text-left">image</th>
            <th class="text-left">操作</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in products" :key="item.denny">
            <td>{{ item.product.name }}</td>
            <td>{{ item.product.price }}</td>
            <td><img :src="item.product.image" style="width: 100px" /></td>
            <td>
              <v-form ref="form">
                <v-text-field
                  v-model="products[index].quantity"
                  type="number"
                  required
                  min="1"
                  @input="updateCart(index, products[index].quantity)"
                >
                </v-text-field>
                <v-btn @click="updateCart(index, 0)">X</v-btn>
              </v-form>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    async updateCart (index, quantity) {
      try {
        await this.api.patch(
          '/users/me/cart',
          { product: this.products[index].product._id, quantity },
          {
            headers: {
              authorization: 'Bearer ' + this.user.token
            }
          }
        )
        if (quantity === 0) {
          this.products.splice(index, 1)
          this.$store.commit('user/updateCart', this.user.cart - 1)
        }
      } catch (error) {
        console.log(error)
        this.$swal({
          icon: 'error',
          title: '失敗',
          text: '修改購物車失敗'
        })
      }
    }
  },
  async created () {
    try {
      const { data } = await this.api.get('/users/me/cart', {
        headers: {
          authorization: 'Bearer ' + this.user.token
        }
      })
      this.products = data.result
    } catch (error) {
      this.$swal({
        icon: 'error',
        title: '失敗',
        text: '取得購物車失敗'
      })
    }
  }
}
</script>
<style>
.container {
  width: 100%;
  padding: 151px;
  margin-right: auto;
  margin-left: auto;
}
</style>
