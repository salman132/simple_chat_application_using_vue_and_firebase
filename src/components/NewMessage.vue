<template>
  <div class="new-message">
    <form @submit.prevent="addMessage">
      <label for="new-message">New Message (enter to add)</label>
      <input type="text" name="newMsg" id="new-message" v-model="newMessage">
      <p class="red-text" v-if="feedback">{{ feedback }}</p>
    </form>
  </div>
</template>

<script>
  import db from '@/firebase/init'
  export default {
      name: 'NewMessage',
      props: ['name'],
      data(){
          return{
            newMessage: null,
            feedback:null,
          }
      },
      methods:{
          addMessage:function () {
            if(this.newMessage){
                this.feedback = null;
              db.collection('messages').add({
                  content: this.newMessage,
                  name: this.name,
                  timestamp: Date.now()
              }).then(()=>{
                  this.newMessage = null;

              }).catch(error =>{
                  console.log(error)
              })
            }
            else{
                this.feedback = 'Please enter the message'
            }
          }
      }
  }
</script>
