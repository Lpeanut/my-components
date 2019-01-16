<template>
  <div>
    <i-form ref="form" :model="formValidate" :rules="ruleValidate">
      <i-form-item label="用户名" prop="name">
        <i-input v-model="formValidate.name"></i-input>
      </i-form-item>
      <br>
      <i-form-item label="邮箱" prop="mail">
        <i-input v-model="formValidate.mail"></i-input>
      </i-form-item>
      <br>
      <button class="button" @click="submit($event, 'form')">SUBMIT</button>
    </i-form>
  </div>
</template>

<script>
import iForm from '../components/form/form.vue'
import iFormItem from '../components/form/form-item.vue'
import iInput from '../components/input/input.vue'
export default {
  components: { iForm, iFormItem, iInput },
  data () {
    return {
      formValidate: {
        name: '',
        mail: ''
      },
      ruleValidate: {
        name: [
          { required: true, message: '用户名不能为空', trigger: 'change' },
          { validator: this.customValidator, trigger: 'blur' }
        ],
        mail: [
          { required: true, message: '邮箱不能为空', trigger: 'blur' },
          { type: 'email', message: '邮箱格式不正确', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    submit (e, formName) {
      e.preventDefault()
      this.$refs[formName].validateForm().then(valid => {
        if (valid) {
          console.log(this.formValidate)
        }
      })
    },
    customValidator (rule, value, callback) {
      const reg = /\d/g
      if (reg.test(value)) {
        return callback(new Error('姓名中不能含有数字'))
      }
      callback()
    }
  }
}
</script>

<style lang="scss">
.button{
  padding: 10px 20px;
  background: #ed6c00;
  border-radius: 5px;
  color: white;
  border: none;
  outline: none;
  transition: all 0.1s linear;
  &:active{
    transform: translateY(2px)
  }
}
</style>
