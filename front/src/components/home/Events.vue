<template lang="html">
  <div class="events">
    <h3>Activities <em>(last {{events.length}})</em></h3>
    <ul>
      <li v-for="event in events" class="event">
        <span class="date">{{event.created_at | date('YYYY-MM-DD HH:mm')}}</span>
        <span class="type">{{getFormatedType(event.type)}} on </span>
        <span class="repo">{{event.repo.name}}</span>
      </li>
      <li v-if="!progress && events.length == 0" class="no_events">
        <span>No events</span>
      </li>
    </ul>
    <v-loader v-if="progress"></v-loader>
  </div>

</template>

<script>

import auth from '../../auth'
import env from 'env'
import Loader from '../Loader.vue'

export default {
  name: 'events',
  data () {
    return {
      events:[],
      progress: false
    }
  },
  components: {
    'v-loader':Loader,
  },
  props: [
    'username'
  ],
  created(){
    this.progress = true
    this.$http.get(env.api+'/events'+auth.urlToken()+'&username='+this.username).then(response => {
      this.events = response.body.data
      this.progress = false
    })
  },
  methods:{
    getFormatedType(type){
      switch (type) {
        case 'PushEvent':
          return 'Push Event'
          break;
        case 'IssuesEvent':
          return 'Issues Event'
          break;
        case 'IssueCommentEvent':
          return 'Issues Comment Event'
          break;
        case 'PullRequestEvent':
          return 'Pull Request Event'
          break;
        case 'WatchEvent':
          return 'Watch Event'
          break;
        case 'MemberEvent':
          return 'Member Event'
          break;
        case 'CreateEvent':
          return 'Create Event'
          break;
        case 'GollumEvent':
          return 'Gollum Event'
          break;
        default:
          return type
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.events{
  background: white;
  text-align: left;
  padding: 10px 30px;
  box-shadow: 0 0 1px rgba(0,0,0,0.25);

  h3{
    em{
      font-size: 12px;
      margin-left: 8px;
    }
  }

  ul{
    margin: 0;
    padding: 0;
    max-height: 400px;
    overflow-y: scroll;
  }
  .no_events{
    text-align: center;
    font-weight: bold;
  }
  .event{
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 25px 0;
    border-bottom: 2px solid #42B983;
    overflow: hidden;

    .date{
      color: grey;
      margin-right: 50px;
    }
    .type{
      font-weight: bold;
      margin-right: 10px;
    }
    .repo{

    }
  }

}
</style>
