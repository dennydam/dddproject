<template>
  <div id="section01">
    <v-row>
      <v-col cols="6" class="content1 mt-10" align="center" justify="center">
        <iframe
          width="560"
          height="315"
          :src="video"
          title="YouTube video player"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </v-col>
      <v-col cols="6" class="worktext">
        <h1>訓練說明</h1>
        <v-card>
          <h1>{{ description }}</h1>
        </v-card>
      </v-col>
      <v-col cols="6" align="center" justify="center">
        <v-btn to="/">回去訓練頁</v-btn>
      </v-col>
    </v-row>

    <v-footer dark padless>
      <v-card
        color="primary"
        flat
        tile
        class="indigo lighten-1 white--text text-center"
      >
        <v-card-text>
          <v-btn
            v-for="icon in icons"
            :key="icon"
            class="mx-4 white--text"
            icon
          >
            <v-icon size="24px">
              {{ icon }}
            </v-icon>
          </v-btn>
        </v-card-text>

        <v-card-text class="white--text pt-0">
          Phasellus feugiat arcu sapien, et iaculis ipsum elementum sit amet.
          Mauris cursus commodo interdum. Praesent ut risus eget metus luctus
          accumsan id ultrices nunc. Sed at orci sed massa consectetur dignissim
          a sit amet dui. Duis commodo vitae velit et faucibus. Morbi vehicula
          lacinia malesuada. Nulla placerat augue vel ipsum ultrices, cursus
          iaculis dui sollicitudin. Vestibulum eu ipsum vel diam elementum
          tempor vel ut orci. Orci varius natoque penatibus et magnis dis
          parturient montes, nascetur ridiculus mus.
        </v-card-text>

        <v-divider></v-divider>

        <v-card-text class="white--text">
          {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  </div>
</template>
<script>
export default {
  data () {
    return {
      icons: ['mdi-facebook', 'mdi-twitter', 'mdi-linkedin', 'mdi-instagram'],
      name: '',
      price: 0,
      description: '',
      image: '',
      sell: false,
      video: '',
      category: '',
      quantity: 0
    }
  },
  methods: {
    addCart () {
      this.$store.dispatch('user/addCart', {
        product: this.$route.params.id,
        quantity: this.quantity
      })
    }
  },
  computed: {
    quantityState () {
      return this.quantity === 0 ? null : this.quantity > 0
    }
  },
  async created () {
    try {
      const { data } = await this.api.get('/products2/' + this.$route.params.id)
      this.name = data.result.name
      this.price = data.result.price
      this.description = data.result.description
      this.image = data.result.image
      this.video = data.result.video
      this.sell = data.result.sell
      this.category = data.result.category
      document.title = `${this.name} | 購物網`
      console.log('555')
    } catch (error) {
      console.log(error)
      this.$router.push('/')
    }
  }
}
</script>
<style>
</style>
