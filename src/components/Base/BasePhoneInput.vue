<template>
  <div
    class="phone-input-container"
    :class="classes"
  >
    <label v-if="label">{{ label }}</label>
    <vue-tel-input
      v-model="state.phone"
      v-maska="['+## ## ########', '+## ## #########']"
      class="phone-input"
      :input-options="{
        placeholder,
        required,
      }"
    />
    <span>
      {{ error }}
    </span>
  </div>
</template>

<script lang="ts">
import {
  computed, defineComponent,
  reactive, watch,
} from 'vue';
import { VueTelInput } from 'vue-tel-input';

export default defineComponent({
  components: { VueTelInput },
  model: {
    prop: 'modelValue',
    event: 'input',
  },
  props: {
    modelValue: { type: String, default: '' },
    label: { type: String, default: '' },
    placeholder: { type: String, default: '' },
    error: { type: String, default: '' },
    block: { type: Boolean },
    required: { type: Boolean },
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const state = reactive({
      phone: props.modelValue || '',
    });

    const classes = computed(() => ({
      block: props.block,
      error: props.error,
      hasData: props.modelValue,
    }));

    watch(state, (newState) => {
      emit('update:modelValue', newState.phone);
    });
    return { state, classes };
  },
});

</script>
<style lang="scss" scoped>
.phone-input-container {
  display: flex;
  flex-direction: column;
  gap: 4px;

  label {
    color: $input-labels;
    font-size: 14px;
    font-weight: 700;
    margin-bottom: 5px;
  }
  &.block {
    width: 100%;
  }
}
.phone-input {
  height: 3rem;
  padding: 1rem;
  font-size: 14px;
  width: 100%;
  &:focus-within,
  &.hasData {
    box-shadow: inset 0 0 0 transparent, 0 0 0 transparent;
    border-color: $primary-button;
    background-color: $transparent-focus-bg-elements;
  }
  &::placeholder {
    color: $text-soft-grey;
    opacity: 1;
  }
}
.phone-input-container input.error {
  border-color: $error;

}
.phone-input input {
  background-color: transparent !important;
  color: $text-dark-grey-2  !important;
  font-size: 14px !important;

  &:focus, &.hasData{ /* Microsoft Edge */
    border-color: $primary-button !important;
    background-color: $transparent-focus-bg-elements !important;
  }

  &::placeholder { /* Chrome, Firefox, Opera, Safari 10.1+ */
    color: $text-soft-grey;
    opacity: 1; /* Firefox */
  }

  &:-ms-input-placeholder {
    /* Internet Explorer 10-11 */
    color: $text-soft-grey;
  }

  &::-ms-input-placeholder {
    /* Microsoft Edge */
    color: $text-soft-grey;
  }

}
.phone-input div:hover {
  background-color: transparent;
}
.vue-tel-input{
  border: 1px solid $border-neutral;
}

.vti__input {
  background-color: transparent !important;
}
input {
  background-color: transparent !important;

}
</style>
