<template>
  <v-form>
    <v-container fluid>
      <v-row>
        <v-col cols="12" sm="6" md="6">
          <v-text-field
            v-model="item.name"
            :error-messages="nameErrors"
            label="name"
            required
            @input="$v.item.name.$touch()"
            @blur="$v.item.name.$touch()"
          />
        </v-col>

        <v-col cols="12" />
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
import has from 'lodash/has'
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'

export default {
  name: 'GreetingForm',
  mixins: [validationMixin],
  props: {
    values: {
      type: Object,
      required: true,
    },

    errors: {
      type: Object,
      default: () => {},
    },

    initialValues: {
      type: Object,
      default: () => {},
    },
  },
  data: () => ({
    name: null,
  }),
  computed: {

    // eslint-disable-next-line
    item() {
      return this.initialValues || this.values
    },

    nameErrors () {
      const errors = []

      if (!this.$v.item.name.$dirty) { return errors }

      has(this.violations, 'name') && errors.push(this.violations.name)

      !this.$v.item.name.required && errors.push('Field is required')

      return errors
    },

    violations () {
      return this.errors || {}
    },
  },
  mounted () {
  },
  methods: {
  },
  validations: {
    item: {
      name: {
        required,
      },
    },
  },
}
</script>
