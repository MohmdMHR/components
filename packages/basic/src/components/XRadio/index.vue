<template>
  <div>
    <inputLabel :label="title" :required="required" :description="description"/>
    <v-radio-group
      v-bind="Object.assign({}, $props, $attrs)"
      v-on="$listeners"
      @change="change"
      :rules="innerRules"
    >
      <v-radio v-for="(item, index) in items" :key="index" v-bind="item"></v-radio>
    </v-radio-group>
  </div>
</template>

<script>
export default {
  name: 'x-radio',
  props: {
    title: { type: String, default: '' },
    description: { type: String, default: '' },
    required: { type: Boolean, default: false },
    errorMessages: { type: String, default: '' },
    items: {
      type: Array,
      default: () => []
    },
    column: {
      type: Boolean,
      default: false
    },
    value: {
      type: String,
      default: ''
    },
    rules: {}, //Array or Function, so no assigned here
    id: { type: String, default: undefined }
  },
  methods: {
    change(val) {
      this.$emit('input', val);
    }
  },
  data: function() {
    return {
      innerRules:
        this.rules ||
        (this.required
          ? [(v) => !!v || `${this.label || 'field'} is required`]
          : [])
    };
  }
};
</script>

<style lang="stylus" scoped>
@import '../../styles/_variables.styl';

.v-input--selection-controls {
  margin-top: 20;

  >>>.v-input__control {
    .v-input--radio-group__input {
      .v-radio--is-disabled { /* disable */
        .v-label {
          color: $silver;
        }

        &.v-radio {
          .v-input--selection-controls__input {
            .v-icon {
              color: $pale-grey !important;
            }
          }
        }
      }

      .v-label {
        color: $slate-grey; /* label */
        font-size: 16px;
      }

      .v-radio {
        .v-input--selection-controls__input {
          .v-icon {
            color: $silver; /* input icon */
          }

          .accent--text {
            color: $secondary-01 !important; /* color active input icon */
          }
        }

        &.accent--text { /* color active label */
          .v-label {
            color: $secondary-01;
          }
        }
      }
    }

    .v-messages { /* error message */
      height: 20px;
      font-size: 14px;

      &.error--text {
        color: $error !important;
        caret-color: $error !important;
      }
    }
  }
}
</style>
