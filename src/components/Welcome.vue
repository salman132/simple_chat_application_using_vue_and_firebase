<template>
  <div class="welcome container ">
    <div class="card">
      <div class="card-content content-align">
        <h2 class="teal-text">Welcome</h2>
        <form @submit.prevent="enterChat">
          <label for="name">Your name</label>
          <input type="text" name="name" id="name" v-model="name" placeholder="Your Name">
          <p v-if="feedback" class="red-text">{{ feedback}}</p>
          <button class="btn teal">Enter Chat</button>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Welcome',
  data () {
    return {
      name: null,
      feedback: null
    }
  },
  methods:{
      enterChat:function () {
        if(this.name){
            this.$router.push({name: 'Chat',params: {name: this.name}})
        }
        else{
            this.feedback = 'Enter the name to join';
        }
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
  .welcome{
    max-width: 400px;
    margin-top: 50px;
  }
  .welcome h2{
    font-size: 3em;
  }
  .welcome button{
    margin: 30px auto;
  }
</style>
