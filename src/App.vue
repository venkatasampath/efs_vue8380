<template>
  <v-app>
    <v-toolbar app class="orange" >
      <v-toolbar-title>Eagle Finance Service</v-toolbar-title>
      <v-spacer></v-spacer>
      <v-toolbar-items>
          <v-btn flat dark @click="goHome">Home
            <v-icon right>home</v-icon>
          </v-btn>
          <v-btn flat dark @click="viewCustomers">Customers
            <v-icon right>people</v-icon>
          </v-btn>
          <v-btn flat dark @click="viewStocks">Stocks
            <v-icon right>stock</v-icon>
          </v-btn>
          <v-btn flat dark @click="viewInvestments">Investments
        
          </v-btn>
          <v-btn flat dark v-if="!authenticated"
                @click="login">Login
          </v-btn>
          <v-btn flat dark v-if="authenticated"
                @click="logout">Log Out
               <v-icon right>exit_to_app</v-icon> 
          </v-btn>
      </v-toolbar-items>
    </v-toolbar>
      <v-content>

      <router-view/>
    </v-content>

  </v-app>
</template>

<script>
import router from './router';
import {APIService} from './http/APIService';
const apiService = new APIService();
export default {
  name: 'App',
  data: () => ({
      authenticated: false,
    }),

 mounted() {
        apiService.getCustomerList().then(response => {
          this.authenticated = true;
        }).catch(error => {
          if (error.response.status === 401) {
            localStorage.removeItem('isAuthenticates');
            localStorage.removeItem('log_user');
            localStorage.removeItem('token');
            this.authenticated = false;
          }
        });
        console.log('this.authenticated--'+this.authenticated);
    },

    methods: {
      logout() {
        localStorage.removeItem('isAuthenticates');
        localStorage.removeItem('log_user');
        localStorage.removeItem('token');
        this.authenticated = false;
       // router.push('/');
         window.location = "/"
      },
      viewCustomers() {
        router.push('/customer-list');
      },
       viewInvestments() {
        router.push('/investment-list');
      },
       viewStocks() {
        router.push('/stock-list');
      },

      login() {
        router.push("/auth");
      },

      goHome() {
        router.push('/');
      }
    }
  };


</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #129ea8;
  margin-top: 40px;
}
</style>
