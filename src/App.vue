<template>
    <div class="container">
      <form class="pt-5" @submit.prevent="onSubmit">
      <div class="form-group">
        <label for="email">Email</label>
        <input type="email"
               id="email"
               class="form-control"
               :class="{'is-invalid': $v.email.$error}"
               @blur="$v.email.$touch()"
               v-model="email">
        <div class="invalid-feedback" v-if="!$v.email.required">Поле email бязательно для ввода</div>
        <div class="invalid-feedback" v-if="!$v.email.email">email введен неправильно</div>
        <div class="invalid-feedback" v-if="!$v.email.uniqEmail">email уже занят</div>
      </div>
      <div class="form-group">
        <label for="password">Password</label>
        <input type="password"
              id="password"
              class="form-control"
              :class="{'is-invalid': $v.password.$error}"
               @blur="$v.password.$touch()"
              v-model="password">
        <div class="invalid-feedback" v-if="!$v.password.required">Поле password бязательно для ввода</div>
        <div class="invalid-feedback" v-if="!$v.password.minLength">Пароль должен быть больше {{$v.password.$params.minLength.min}} символов. Сейчас введено {{password.length}}</div>
      </div>
      <div class="form-group">
        <label for="confirmPassword">Confirm Password</label>
        <input type="password"
               id="confirmPassword"
               class="form-control"
               :class="{'is-invalid': $v.confirmPassword.$error}"
               @blur="$v.confirmPassword.$touch()"
               v-model="confirmPassword">
        <div class="invalid-feedback" v-if="!$v.confirmPassword.sameAs">Пароли должны быть идентичны</div>
      </div>
      <button class="btn btn-success"
              type="submit"
              :disabled="$v.$invalid"
      >Submit</button>

      </form>
<!--      <pre>{{$v.email}}</pre>-->
    </div>
</template>

<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

export default {
  data() {
    return {
      email: '',
      password: '',
      confirmPassword: ''
    }
  },
  methods: {
    onSubmit() {
      console.log('Email:', this.email)
      console.log('Password:', this.password)
    }
  },
  validations: {
    email: {
      required,
      email,
      uniqEmail: function(newEmail) {
        if (newEmail === '') return true
        return new Promise((resolve, reject) => {
          setTimeout(() => {
            const value = newEmail !== 'test@mail.ru'
            resolve(value)
          }, 3000)

        });
      }
    },
    password: {
      required,
      minLength: minLength(6)
    },
    confirmPassword: {
      required,
      sameAs: sameAs('password')
    }
  }
}

</script>

<style>

</style>
