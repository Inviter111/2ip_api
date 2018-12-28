<template>
  <div>
    <b-container class="bv-example-row">
      <b-row>
        <b-col align-self="center">
          <b-tabs>
            <b-tab title="Гео-IP">
              <b-card>
                <b-container class="bv-example-row">
                  <b-row align-h="between">
                    <b-col>
                      <b-form @submit.prevent="GeoApi">
                        <b-form-group label="Введите IP" label-for="ipInput">
                          <b-form-input id="ipInput" type="number" v-model="ip[0]" min="0" max="255" class="d-inline-block" required/>
                          .
                          <b-form-input id="ipInput" type="number" v-model="ip[1]" min="0" max="255" class="d-inline-block" required/>
                          .
                          <b-form-input id="ipInput" type="number" v-model="ip[2]" min="0" max="255" class="d-inline-block" required/>
                          .
                          <b-form-input id="ipInput" type="number" v-model="ip[3]" min="0" max="255" class="d-inline-block" required/>
                        </b-form-group>
                        <b-button type="submit" variant="primary">Проверить</b-button>
                        <b-button variant="primary" @click="MyIp">Мой IP</b-button>
                      </b-form>
                    </b-col>
                    <b-col>
                      <label><strong>IP: </strong>{{ geoIp.ip }}</label>
                      <label><strong>Страна: </strong>{{ geoIp.country_rus }}</label>
                      <label><strong>Регион: </strong>{{ geoIp.region_rus }}</label>
                      <label><strong>Город: </strong>{{ geoIp.city_rus }}</label>
                      <label><strong>Широта: </strong>{{ geoIp.latitude }}</label>
                      <label><strong>Долгота: </strong>{{ geoIp.longitude }}</label>
                      <label><strong>Индекс: </strong>{{ geoIp.zip_code }}</label>
                      <label><strong>Часовой пояс: </strong>{{ geoIp.time_zone }}</label>
                    </b-col>
                  </b-row>
                </b-container>
              </b-card>
            </b-tab>
          </b-tabs>
        </b-col>
      </b-row>
    </b-container>
    <b-container id="ip-list" class="bv-example-row" v-if="history.length > 0">
      <b-row>
        <b-col align-self="center">
          <div role="tablist" v-for="(item, index) in history" :key="index">
            <b-card no-body class="mb-1">
              <b-card-header class="p-1" role="tab">
                <b-btn block href="#" v-b-toggle="'accordion-' + index" variant="info">{{ item.ip }}</b-btn>
              </b-card-header>
              <b-collapse :id="'accordion-' + index" accordion="accord" role="tabpanel">
                <b-card-body>
                  <p class="card-text">
                    <label><strong>IP: </strong>{{ item.ip }}</label>
                    <label><strong>Страна: </strong>{{ item.country_rus }}</label>
                    <label><strong>Регион: </strong>{{ item.region_rus }}</label>
                    <label><strong>Город: </strong>{{ item.city_rus }}</label>
                    <label><strong>Широта: </strong>{{ item.latitude }}</label>
                    <label><strong>Долгота: </strong>{{ item.longitude }}</label>
                    <label><strong>Индекс: </strong>{{ item.zip_code }}</label>
                    <label><strong>Часовой пояс: </strong>{{ item.time_zone }}</label>
                  </p>
                </b-card-body>
              </b-collapse>
            </b-card>
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
export default {
  mounted() {
    const storage = localStorage
    for (let key in storage) {
      if (/^(?!0)(?!.*\.$)((1?\d?\d|25[0-5]|2[0-4]\d)(\.|$)){4}$/.test(key)) {
        this.history.push(JSON.parse(storage[key]))
      }
    }
  },
  data () {
    return {
      ip: ['', '', '', ''],
      history: [],
      geoIp: {
        ip: '',
        country_code: '',
        country: '',
        country_rus: '',
        region: '',
        region_rus: '',
        city: '',
        city_rus: '',
        latitude: '',
        longitude: '',
        zip_code: '',
        time_zone: ''
      }
    }
  },
  methods: {
    async GeoApi() {
      const res = await this.axios.get(`https://api.2ip.ua/geo.json?ip=${this.ip[0]}.${this.ip[1]}.${this.ip[2]}.${this.ip[3]}`)
      this.geoIp = res.data
      localStorage.setItem(this.geoIp.ip, JSON.stringify(this.geoIp))
      this.history.push(this.geoIp)
    },
    async MyIp() {
      const res = await this.axios.get('https://api.2ip.ua/geo.json?ip=')
      this.geoIp = res.data
      localStorage.setItem(this.geoIp.ip, JSON.stringify(this.geoIp))
      this.history.push(this.geoIp) 
    }
  },
}
</script>

<style scoped>
#ipInput::-webkit-outer-spin-button,
#ipInput::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

#ipInput {
  -moz-appearance: textfield;
  width: 55px;
}

label {
  display: block;
}

#ip-list {
  margin-top: 1em;
}
</style>

