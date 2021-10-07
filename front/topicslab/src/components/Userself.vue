<template>
  <div>
    <Card>
      <template #title>
        mypage
      </template>
      <template #content>
        {{user.name}}
      </template>
      <template #footer>
        <Button label="Create Topic" v-on:click="toNewTopic" />
        <Button label="Logout" class="p-button-warning" v-on:click="logout" />
        <Button label="Withdraw" class="p-button-danger" v-on:click="withdraw" />
      </template>
    </Card>
    <Tab /> 
  </div>
</template>

<script>
import axios from '@/supports/axios'
import Tab from '@/components/Tab'

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
