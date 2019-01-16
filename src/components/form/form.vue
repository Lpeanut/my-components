<template>
  <form>
    <slot></slot>
  </form>
</template>

<script>
export default {
  name: 'iForm',
  provide () {
    return {
      form: this
    }
  },
  props: {
    model: {
      type: Object
    },
    rules: {
      type: Object
    }
  },
  data () {
    return {
      fields: []
    }
  },
  methods: {
    validateForm () {
      return new Promise((resolve, reject) => {
        let valid = true
        this.fields.forEach(item => {
          item.validate('', error => {
            console.log(error)
            if (error) valid = false
          })
        })
        console.log(valid)
        resolve(valid)
      })
    }
  },
  created () {
    this.$on('on-form-item-add', (field) => {
      if (field) this.fields.push(field)
    })
    this.$on('on-form-item-remove', (field) => {
      if (field.prop) this.fields.splice(this.fields.indexOf(field), 1)
    })
  }
}
</script>

<style>

</style>
