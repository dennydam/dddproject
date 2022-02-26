<template>
  <v-app>
    <!-- <v-data-table class="table" :items="products2" :fields='fields' :items-per-page="5">
    </v-data-table> -->
    <v-dialog
      id="modal-product"
      v-model="dialog1"
      persistent
      @click="resetForm"
      max-width="600"
      hide-overlay="false"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          class="additem"
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
          width="200px"
        >
          Open Dialog
        </v-btn>
      </template>
      <v-card>
        <v-form ref="form">
          <v-text-field
            label="商品名稱"
            v-model="form.name"
            type="text"
            required
            placeholder="請輸入商品名稱"
            :state="state.name"
            outlined
          >
          </v-text-field>
        </v-form>

        <v-form ref="form">
          <v-textarea
            v-model="form.description"
            :state="state.description"
            required
            rows="3"
            max-rows="6"
            placeholder="請輸入商品說明"
            color="teal"
          >
          </v-textarea>
        </v-form>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="submitModal"
            :disabled="modalSubmitting"
          >
            送出
          </v-btn>
          <v-btn color="green darken-1" text @click="dialog1 = false">
            取消
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
   <!-- card -->
   <!-- <v-btn align="left" class="flex " v-for="item in products2" :key="item._id" :value="item" @click="discussOpen(item)">
          <div class="border">
            <div class="row col-6 justify-between ">

              <p class="col-12 title">{{ item.name }}</p>

              <p class="des">{{ item.name }}</p>
            </div>
            <div class="flex row col-12 qus_card_bottom">
              <p class="name" style="margin-right:auto">{{ products2.name  }}</p>
              <p class="fb_length">{{ products2.name }}</p>
            </div>
          </div>
        </v-btn> -->
    <!-- table -->
   <v-container mt-15>
         <v-row>
                <v-col
                  cols="12"
                  md="6"
                  lg="3"
                  xl="3"
                  v-for="(item, index) in products2"
                  :key="item._id"
                >

                  <v-card max-width="250">
                      <v-card-title v-text="item.name"></v-card-title>
                    <v-card-text v-text="products2[index].description"> </v-card-text>
                    <v-divider class="mx-4"></v-divider>
                    <v-card-actions>
                      <v-spacer></v-spacer>
                      <v-btn
                        @click ="discussopen(item)">cc
                      </v-btn>
                    </v-card-actions>
                  </v-card>
                   <v-dialog
                   max-width="600"
                    v-model="discuss"
                      >
                    <v-card
                    >
                    <v-card-text>
                    <h1>主題:{{ form.name }}</h1>
                    <v-divider class="mx-4 my-4"></v-divider>
                     <p>描述:{{ form.description }}</p>
                     <v-divider class="mx-4 my-4"></v-divider>
                     <p>{{ user.account }}</p>
                     </v-card-text>
                     <v-text-field
                       v-model="text"
                       outlined
                       append-outer-icon="mdi-send"
                       @click:append-outer="sendmessage()"
                       >

                     </v-text-field>
                    </v-card>

                       </v-dialog>

                </v-col>

              </v-row>
              </v-container>

    <!-- <v-simple-table>
    <template v-slot:default>
      <thead>
        <tr>
          <th class="text-left">
            Name
          </th>
          <th class="text-left">
            Calories
          </th>
          <th class="text-left">
            image
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in products"
          :key="item.denny"
        >
          <td>{{ item.name }}</td>
          <td>{{ item.price }}</td>
          <td><img :src="item.image" style="width:100px"></td>
          <td> </td>
        </tr>
      </tbody>
    </template>
  </v-simple-table> -->
  </v-app>
</template>
<script>
export default {
  data () {
    return {
      sending: false,
      text: '',
      modalSubmitting: false,
      products2: [],
      form: {
        name: '',
        description: '',
        _id: ''
      },
      // editProduct (index) {
      //   this.form = {
      //     name: this.products[index].name,
      //     price: this.pooducts[index].price,
      //     image: null,
      //     sell: false,
      //     description: '',
      //     category: '',
      //     _id: ''
      //   }
      //   this.$bvModal.show('modal-product')
      // },
      discuss: false,
      dialog: false,
      dialog1: false,
      dialogDelete: false,
      editedIndex: -1
    }
  },
  computed: {
    state () {
      return {
        name: this.form.name.length === 0 ? null : true
      }
    }
  },

  created1 () {
    this.initialize()
  },

  methods: {
    async submitModal () {
      if (!this.state.name) {
        return
      }
      this.modalSubmitting = true

      const fd = new FormData()
      for (const key in this.form) {
        if (key !== '_id') {
          fd.append(key, this.form[key])
        }
      }

      try {
        if (this.form._id.length === 0) {
          const { data } = await this.api.post('/board', fd, {
            headers: {
              authorization: 'Bearer ' + this.user.token
            }
          })
          this.products2.push(data.result)
          this.dialog1 = false
        } else {
          await this.api.patch('/board/' + this.form._id, fd, {
            headers: {
              authorization: 'Bearer ' + this.user.token
            }
          })
          this.products2[this.form.index] = {
            ...this.form
          }
        }
        this.modalSubmitting = false
        this.form = {
          name: '',
          description: '',
          _id: ''
        }
      } catch (error) {
        this.$swal({
          icon: 'error',
          title: '錯誤',
          text: error.response.data.message
        })
      }
    },
    discussopen (item) {
      console.log(item)
      this.discuss = true
      this.form.name = item.name
      this.form._id = item._id
      this.form.description = item.description
      console.log(this.products2)
      // this.form = item
    },
    async sendmessage () {
      try {
        const { data } = await this.api.post('/board/' + this.form._id, { text: this.text, account: this.user.account }, {
          headers: {
            authorization: 'Bearer ' + this.user.token
          }
        })
        console.log(data)
        this.discuss = false
      } catch (error) {
        console.log(error)
      }
      this.sending = false
      this.text = ''
    }
  },
  async created () {
    try {
      const { data } = await this.api.get('/board/all', {
        headers: {
          authorization: 'Bearer ' + this.user.token
        }
      })
      this.products2 = data.result
    } catch (error) {
      console.log(error)
      this.$swal({
        icon: 'error',
        title: '錯誤',
        text: '取得商品失敗'
      })
    }
  }
}
</script>
<style>
.table {
  margin-top: 500px;
}

.container {
  max-width: 1000px;
}

.additem {
  position: absolute;
  left: 620px;
  top: 25px;
  z-index: 22;
}
</style>
