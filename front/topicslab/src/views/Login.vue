<template>
  <div>
    <Card>
      <template #title>
        ログイン
      </template>
      <template #content>
        <div class="fields">
          <div class="p-field">
            <label for="email">メールアドレス</label>
            <InputText id="email" type="email" v-model="email" />
          </div>
          <div class="p-field">
            <label for="password">パスワード</label>
            <InputText id="password" type="password" v-model="password" />
          </div>
        </div>
        <span class ="error">
          <dialog open>{{message}}
            <button id="close" type="button">close</button>
          </dialog>
        </span>
        <div class="p-field">
          <Button icon="pi pi-check" label="Login" v-on:click="login" />
        </div>
        <div>
          <a href="Register">新規会員登録はこちら</a>
        </div>
      </template>
    </Card>
  </div>
</template>

<script>
import axios from '@/supports/axios'

export default {
  name: 'Login',
  data () {
    return {
      email: '',
      password: '',
      error: false,
      message: ''
    }
  },
  methods: {
    login () {
      axios.get('/sanctum/csrf-cookie')
        .then(() => {
          axios.post('/api/login', {
            email: this.email,
            password: this.password
          })
            .then((res) => {
              if (res.status === 200) {
                console.log('ログイン成功')
                localStorage.setItem('authenticated', 'true')
                location.href = '/'
              } else {
                this.message = 'ログインに失敗しました。'
                const modal = document.querySelector('dialog')
                modal.showModal()
                modal.close('Accepted')
                modal.addEventListener('click', (event) => {
                  if (event.target === modal) {
                    modal.close('cancelled')
                  }
                })
              }
            })
            .catch((err) => {
              console.log(err)
              this.message = 'ログインに失敗しました。'
            })
        })
        .catch((err) => {
          alert(err)
        })
    }
  }
}
/** const modal = document.querySelector('dialog')
modal.showModal()
modal.close() */
</script>

<style lang="scss" scoped>
.p-card-content {
  .fields {
    text-align: center;
  }

  .p-field {
    display: block;

    label {
      display: inline-block;
      width: 10em;
      margin-bottom: 10px;
    }

    .p-button {
      margin-top: 20px;
      display: block;
      width: 100%;
    }
  }
}
.error {
  color: red
}
dialog {
  padding: 0;
  border: 0;
  border-radius: 0.6rem;
  box-shadow: 0 0 1em black;
}
dialog::backdrop {
  background-color: rgba(0, 0, 0, 0.4)
}

</style>
