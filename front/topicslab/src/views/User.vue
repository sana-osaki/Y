<template>
  <div>
    <Card>
      <template #content>
       <!-- {{user.name}} -->
        <!-- {{user.topics}} -->
        <!-- {{user.comments.id}} -->
        <!-- {{topics}} -->
        <!-- {{user.comments}} -->
        <!-- {{comments}} -->
      </template>
      <TabView>
        <TabPanel header="Header I">{{user.topics}}</TabPanel>
        <!-- <TabPanel header="Header I">トピック{{user.topics}}</TabPanel> -->
        <!-- <TabPanel header="Header I">トピック{{user.topic}}</TabPanel> -->
        <TabPanel header="Header I">トピック</TabPanel>
      </TabView>
    </Card>
  </div>
  <!-- <Tab /> -->
 <meta name="viewport" content="width=device-width, initial-scale=1">
    <div class="tab">
      <button class="tablinks" onclick="openTopic(event, 'Topic1')">Topic1</button>
      <button class="tablinks" onclick="openTopic(event, 'Topic2')">Topic2</button>
      <button class="tablinks" onclick="openTopic(event, 'Topic3')">Topic3</button>
    </div>
    <div id="Topic1" class="tabcontent">
      <h3>トピック1</h3>
      <p>Topic1の内容がここに表示されます。</p>
    </div>
    <div id="Topic2" class="tabcontent">
      <h3>トピック2</h3>
      <p>Topic2の内容がここに表示されます。</p>
    </div>
    <div id="Topic3" class="tabcontent">
      <h3>トピック3</h3>
      <p>Topic3の内容がここに表示されます。</p>
    </div>
</template>

<script>
import axios from '@/supports/axios'

export default {
  name: 'user',
  data () {
    return {
      id: null,
      user: {}
    }
  },

  mounted () {
    if (localStorage.getItem('authenticated') !== 'true') {
      this.$router.push('/login')
      return
    }
    this.id = this.$route.params.id
    if (!this.id) {
      alert('不正なIDです。')
    }
    this.getUser()
  },

  methods: {
    getUser () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.get(`/api/user/${this.id}`)
            .then((res) => {
              console.log(res)
              if (res.status === 200) {
                this.user = res.data
              } else {
                console.log('取得失敗')
              }
            })
            .catch((err) => {
              console.log(err)
            })
        })
        .catch((err) => {
          alert(err)
        })
    }
  }
}
</script>

<style>
  .tab {
  overflow: hidden;
  border: 1px solid #ccc;
  background-color: #f1f1f1;
}
.tab button {
  background-color: inherit;
  float: left;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 14px 16px;
  transition: 0.3s;
}
.tab button:hover {
  background-color: #ddd;
}
.tab button.active {
  background-color: #ccc;
}
.tabcontent {
  display: none;
  padding: 6px 12px;
  border: 1px solid #ccc;
  border-top: none;
}
</style>
