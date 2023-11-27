<template>
  <div class="limiter">
    <div class="container">
      <h2>{{ title }}</h2>
      <div class="auth" v-if="state == 'login'">
        <label for="email">Email</label>
        <input v-model="data.email" type="email" required />
        <label for="password">Password</label>
        <input v-model="data.password" type="password" required />
        <button @click="handleLogin()" type="submit">Login</button>
      </div>
      <form v-else @submit.prevent="handleRegister()">
        <label for="username">Username</label>
        <input v-model="data.username" type="text" required />
        <label for="email">Email</label>
        <input v-model="data.email" type="email" required />
        <label for="password">Password</label>
        <input v-model="data.password" type="password" required />
        <button type="Submit">Register</button>
      </form>
      <h3 v-if="state == 'register'">Have an account? <a @click="toggle('login')">SignIn</a></h3>
      <h3 v-else>Don't have an account? <a @click="toggle('register')">Register</a></h3>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
export default {
  name: 'LoginItem',
  data() {
    return {
      data: {
        username: '',
        email: '',
        password: ''
      },
      state: 'login'
    }
  },
  computed: {
    title() {
      return this.state == 'login' ? 'Login' : 'Register'
    }
  },
  methods: {
    toggle() {
      if (this.state == 'login') {
        this.state = 'register'
      } else {
        this.state = 'login'
      }
    },
    handleRegister() {
      console.log(this.data)
      axios
        .post('http://localhost:3000/user/', this.data)
        .then((res) => {
          console.log(res)
          alert('user successfully registered')
        })
        .catch((error) => {
          console.log(error)
        })
    },
    handleLogin() {
      axios
        .post('http://localhost:3000/user/login', this.data)
        .then((res) => {
          localStorage.setItem('token', res.data.token)
          console.log(res)
          if (res.status == 200) {
            this.$router.push('/allPosts')
          }
        })
        .catch((error) => {
          console.log(error)
        })
    }
    // login() {
    //   const userData = {
    //     username: this.username,
    //     password: this.password
    //   }
    //   console.log(userData)
    //   axios
    //     .post('http://localhost:3000/user/login', userData)
    //     .then((response) => {
    //       console.log(userData)
    //       localStorage.setItem('token', response.data.token)
    //       console.log(response.data)
    //       // axios.get('http://localhost:3000/post').then((response) => {
    //       //   console.log(response)
    //       // })
    //     })
    //     .catch((error) => {
    //       console.error(error)
    //     })
    // }
  }
}
</script>

<style scoped>
.container {
  max-width: 350px;
  margin: 0 auto;
  background: #fff;
  padding: 20px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.2);
  border-radius: 5px;
  margin-top: 100px;
}

h2 {
  text-align: center;
}

form,
div.auth {
  /* width: 80%; */
  display: flex;
  flex-direction: column;
  /* gap: 20px; */
  border-radius: 20px;
}

.container form label,
.container form input,
div .auth input,
div .auth label {
  margin-bottom: 10px;
}

.container form input[type='text'],
.container form input[type='email'],
.container form input[type='password'],
div .auth input[type='email'],
div .auth input[type='password'] {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 10px;
}

form button,
div button {
  background: #007bff;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.container form button:hover {
  background: #0056b3;
}

/* form input,
div input {
  padding: 10px;
} */

h3 {
  padding: 5px;
}

h3 a:hover {
  color: blue;
  cursor: pointer;
}

/* form button,
div button {
  width: 40%;
  padding: 20px;
  background-color: rgb(92, 146, 10);
} */
</style>
