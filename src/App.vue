<template>
  <div>
    <div id='G_OAuth_btn'></div>
    <button v-if="Object.keys(user).length != 0" @click="handleSignOut">Sign Out</button>
    <div v-if="user">
        <img :src="user.picture" />
        <h3>{{user.name}}</h3>
    </div>
  </div>
</template>

<script>
import VueJwtDecode from 'vue-jwt-decode'

export default {
  name: 'App',
  data(){
    return {
      user: {}, 
    }
  }, 
  mounted(){
    let google = window.google;
    google.accounts.id.initialize({
      client_id: '클라이언트 ID', 
      callback: this.handleCallbackResponse
    })
    google.accounts.id.renderButton(
      document.getElementById('G_OAuth_btn'), 
      { theme: 'outline', size: 'large' }
    )
  }, 
  methods: {
    handleCallbackResponse(res){
      console.log('Encoded JWT ID token: ' + res.credential);

      let userObject = VueJwtDecode.decode(res.credential);
      console.log(userObject);
      this.user = userObject;
      document.querySelector('#G_OAuth_btn').hidden = true;

    }, 
    handleSignOut(){
      this.user = {};
      document.querySelector('#G_OAuth_btn').hidden = false;
    }
  }
}
</script>

<style>
</style>
