<template>
  <div>
    <Card>
      <template #title>
        マイぺージ
      </template>
      <template #content>
        <!-- {{user.name}} -->
        {{user.topics}}
      </template>
      <template #footer>
        <Button label="トピック作成" v-on:click="toNewTopic" />
        <Button label="ログアウト" class="p-button-warning" v-on:click="logout" />
        <Button label="退会" class="p-button-danger" v-on:click="withdraw" />
      </template>
      <Tab2 />
    </Card>
  </div>
<div>
<meta name="viewport" content="width=device-width, initial-scale=1">
    <div class="tab">
      <button class="tablinks" onclick="openTopic(event, 'Topic1')">トピック１</button>
      <button class="tablinks" onclick="openTopic(event, 'Topic2')">トピック２</button>
      <button class="tablinks" onclick="openTopic(event, 'Topic3')">トピック３</button>
    </div>
    <div id="Topic1" class="tabcontent">
      <h3>Topic1</h3>
      <p>Topic1の内容がここに表示されます。</p>
    </div>
    <div id="Topic2" class="tabcontent">
      <h3>Topic2</h3>
      <p>Topic2の内容がここに表示されます。</p>
    </div>
    <div id="Topic3" class="tabcontent">
      <h3>Topic3</h3>
      <p>Topic3の内容がここに表示されます。</p>
    </div>
  </div>
</template>

<script>
import axios from '@/supports/axios'

export default {
  name: 'Userself',
  data () {
    return {
      user: {}
    }
  },
  mounted () {
    if (localStorage.getItem('authenticated') !== 'true') {
      this.$router.push('login')
      return
    }

    this.getUser()
  },
  methods: {
    toNewTopic () {
      this.$router.push('topic')
    },
    logout () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.post('/api/logout')
            .then(res => {
              console.log(res)
              localStorage.setItem('authenticated', 'false')
              this.$router.push('/login')
            })
            .catch(err => {
              console.log(err)
            })
        })
        .catch((err) => {
          alert(err)
        })
    },
    withdraw () {
      //
    },
    getUser () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.get('/api/user')
            .then((res) => {
              if (res.status === 200) {
                this.user = res.data
              } else {
                console.log('取得失敗')
              }
            })
        })
        .catch((err) => {
          alert(err)
        })
    }
  }
}
function openTopic (evt, TopicNumber) {// eslint-disable-line
  var i, tabcontent, tablinks
  tabcontent = document.getElementsByClassName('tabcontent')
  for (i = 0; i < tabcontent.length; i++) {
    tabcontent[i].style.display = 'none'
  }
  tablinks = document.getElementsByClassName('tablinks')
  for (i = 0; i < tablinks.length; i++) {
    tablinks[i].className = tablinks[i].className.replace(' active', '')
  }
  document.getElementById(TopicNumber).style.display = 'block'
  evt.currentTarget.className += 'active'
}
</script>

<style lang="scss" scoped>
.p-card-footer {
  .p-button {
    margin-right: 10px;
  }
}
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
