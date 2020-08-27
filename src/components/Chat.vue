<template>
  <div class="chat container">
    <h2 class="center teal-text">Chat</h2>
    <div class="card">
      <div class="card-content">
        <ul class="messages" v-chat-scroll>
          <li v-if="messages" v-for="(msg, index) in messages" :key="index">
            <span class="teal-text">{{ msg.name}}</span>
            <span class="grey-text text-darken-3">{{ msg.content }}</span>
            <span class="grey-text time">{{ msg.timestamp }}</span>
          </li>
        </ul>
      </div>
      <div class="card-action">
        <NewMessage :name="name"></NewMessage>

      </div>
    </div>
  </div>
</template>

<script>
  import NewMessage from "@/components/NewMessage";
  import db from "@/firebase/init";
  import moment from 'moment'

  export default {
      name: 'Chat',
      props: ['name'],

      components:{
          NewMessage: NewMessage,
      },
      data(){
          return{
              messages:[],
          }
      },
      created() {
          this.fetchMessages();
      },
      methods:{
          fetchMessages:function () {
              let ref = db.collection('messages').orderBy('timestamp')
              ref.onSnapshot(snapshot => {
                  snapshot.docChanges().forEach(change =>{
                      if(change.type == 'added'){
                          let doc = change.doc
                          this.messages.push({
                              id:doc.id,
                              name: doc.data().name,
                              content: doc.data().content,
                              timestamp: moment(doc.data().timestamp).format('lll'),

                          })
                      }
                  })
              })

          }
      }
  }
</script>

<style>
  .chat{
    max-width: 40%;
  }
  .chat h2{
    font-size: 2.6em;
    margin-bottom: 40px;
  }
  .chat span{
    font-size: 1.4em;
  }
  .chat .time{
    display: block;
    font-size: 0.8em;
  }
  .chat ul li{
    text-align: left;
  }
  .messages{
    max-height: 300px;
    overflow: auto;
  }
  .messages::-webkit-scrollbar{
    width: 3px;
  }
  .messages::-webkit-scrollbar-track{
    background: #aaa;
  }
  .messages::-webkit-scrollbar-thumb{
    background: #aaa;
  }

</style>
