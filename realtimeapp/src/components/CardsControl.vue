<template>
<v-app id="chat1">
  <v-container>
    <v-layout row wrap justify-center>
      <v-flex xs12 md6 lg4 xl3>
        <v-layout row wrap justify-center>
          <v-flex xs12 sm5>
            <span class="text-xs-left">Altura do cartão é: {{ msg }}</span>
          </v-flex>
          <v-flex xs12 sm6 v-bind:style="{ marginTop: top + 'px' }">
            <v-card>
              <v-card-title primary-title>
                <div>
                  <h3 class="headline mb-0 text-xs-left">Primeiro card</h3>
                  <div class="text-xs-left">Escutando na sala CHAT</div>
                </div>
              </v-card-title>
            </v-card>
          </v-flex>
        </v-layout>
      </v-flex>
      <v-flex xs12 md6 lg4 xl3>
        <v-layout row wrap justify-center>
          <v-flex xs12 sm5>
            <span class="text-xs-left">Altura do cartão é: {{ msg1 }}</span>
          </v-flex>
          <v-flex xs12 sm6 v-bind:style="{ marginTop: top1 + 'px' }">
            <v-card>
              <v-card-title primary-title>
                <div>
                  <h3 class="headline mb-0 text-xs-left">Segundo card</h3>
                  <div class="text-xs-left">Escutando na sala CHAT1</div>
                </div>
              </v-card-title>
            </v-card>
          </v-flex>
        </v-layout>
      </v-flex>
    </v-layout>
  </v-container>
  <v-tooltip left>
    <v-btn fab dark large color="purple" id="button" slot="activator" @click="controle">
      <v-icon dark>videogame_asset</v-icon>
    </v-btn>
    <span>Acessar controle de cards</span>
  </v-tooltip>
</v-app>
</template>

<script>
import io from 'socket.io-client'
export default {
  name: 'CardsControl',
  data: () => ({
    msg: 'Aguardando...',
    msg1: 'Aguardando...',
    isStart: true,
    // socket: io('https://websocketservice.vinicius.rs/socket.io/'),
    socket: io('http://localhost:5000/socket.io/'),
    top: '15',
    top1: '15'
  }),
  methods: {
    controle () {
      window.open('/controle', '_blank', 'toolbar=yes,scrollbars=yes,resizable=yes,top=500,left=500,width=500,height=700')
    }
  },
  mounted () {
    var that = this
    that.socket.on('connect', function (val) {
      setTimeout(() => {
        that.socket.emit('join', 'chat', function () {})
      }, 2000)
      setTimeout(() => {
        that.socket.emit('join', 'chat1', function () {})
      }, 2000)

      that.socket.on('message', function (val) {
        switch (val.room) {
          case 'chat':
            that.top = val.msg
            that.msg = val.msg
            break
          case 'chat1':
            that.top1 = val.msg
            that.msg1 = val.msg
            break
          default:
        }
      })
    })
  }
}
</script>

<style scoped>
#button {
  position: fixed;
  bottom: 10px;
  right: 10px;
}
</style>
