<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
     <p className="App-link">
      Before loggin:
    </p>

    <button v-on:click="mingleInit">Init Mingle</button>
    <button v-on:click="anonymousMetrics">Anonymous Metrics</button>
    <button v-on:click="auth">Loggin</button>

    <p className="App-link">
      After loggin:
    </p>

    <button v-on:click="configMingleURL">Config Mingle URL</button>
    <button v-on:click="gateway">gateway</button>
    <button v-on:click="getAccessToken">Get Access Token</button>
    <button v-on:click="getAllItemsFromStorage">Get All Items From Storage</button>
    <button v-on:click="getBodyToRefreshTokenAPI">Get Body To Refresh Token API</button>
    <button v-on:click="getRefreshTokenURL">Get Refresh Token URL</button>
    <button v-on:click="getSessionInfo">Get Session Info</button>
    <button v-on:click="logout">Logout</button>
  </div>
</template>

<script>

import { MingleService, Configuration } from '@totvs/mingle';

const config = new Configuration();
const mingleService = new MingleService();

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  methods: {
    mingleInit: function () {
      config.app_identifier = 'your_app_id';
      config.environment = 'DEV';
      config.server = 'https://hom-mingle.totvs.com.br/api';
      config.modules.crashr = true;
      config.modules.usage_metrics = true;
      config.modules.gateway = true;
      config.modules.push_notification = true;
      config.modules.ocr = true;
      config.modules.web = true;

      mingleService.setConfiguration(config);
      mingleService.init().then(res => {
        console.log('Mingle init is ok.');
      })
    },
    anonymousMetrics: function () {
      mingleService.registerMetric('ANONYMOUS METRICS');
    },
    auth: function () {
      mingleService.auth.login('login', 'password', 'ALIAS').subscribe(() => {
        console.log('Logged in Mingle.');
        mingleService.registerMetric('LOGIN SUCCESS');
      },
      (authError) => {
        console.log('error in auth Mingle: ', authError.response.data);
      });
    },
    configMingleURL: function () {
      console.log(mingleService.configMingleURL('check_security'));
    },
    gateway: function () {
      const url = 'check_security';

      mingleService.gateway.get(url).subscribe(res => {
        console.log(res);
      }, err => {
        console.log('error: ', err);
      });
    },
    getAccessToken: function () {
      console.log(mingleService.getAccessToken());
    },
    getAllItemsFromStorage: function () {
      mingleService.getAllStorage().then(res => {
        console.log(res);
      });
    },
    getBodyToRefreshTokenAPI: function () {
      console.log(mingleService.getBodyToRefreshTokenAPI());
    },
    getRefreshTokenURL: function () {
      console.log(mingleService.getRefreshTokenURL());
    },
    getSessionInfo: function () {
      console.log(mingleService.getSessionInfo());
    },
    logout: function () {
      mingleService.auth.logout().subscribe(() => {
        // console.log('Loggout ok.');
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
