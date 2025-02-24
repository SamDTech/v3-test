<template>
  <form
    v-if="!isCompleted"
    class="form"
  >
    <div class="form-content">
      <div class="inputs">
        <base-input
          v-model="v$.email.$model"
          :error="v$.email.$errors[0]?.$message"
          :label="t(`CheckEmail.form.email`)"
          :placeholder="t(`CheckEmail.form.emailPlaceholder`)"
          type="email"
        />
      </div>
      <div class="buttons">
        <div>
          <base-button
            :disabled="v$.$invalid"
            primary
            block
            @click.prevent="handleCheckEmail"
          >
            {{ t(`CheckEmail.form.sendMailButton`) }}
          </base-button>
        </div>
      </div>
    </div>
  </form>
  <completed v-else />
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';
import { useI18n } from 'vue-i18n';
import { useStore } from 'vuex';
import useVuelidate from '@vuelidate/core';
import { RulesType } from '@/types/Vuelidate';
import { required, email } from '@/utils/I18nValidators';

import Completed from './Completed.vue';
import { CheckEmailType } from '@/store/modules/auth';
import useToast from '@/hooks/useToast';

export default defineComponent({
  components: { Completed },
  props: {
    isCompleted: { type: Boolean, required: true },
  },
  setup(props, { emit }) {
    const store = useStore();
    const { t } = useI18n();
    const toast = useToast();

    const form = computed<CheckEmailType>(() => store.getters['auth/getCheckEmailData']);

    const rules = {
      email: { required, email },
    } as RulesType;

    const v$ = useVuelidate(rules, form.value);

    async function handleCheckEmail() {
      const isValidate = await v$.value.$validate();

      if (isValidate) {
        form.value.language = window.localStorage.getItem('lang') || 'en-US';
        const {data, error} = await store.dispatch('auth/submitCheckEmail');

        if (data) {
          emit('setCompleted', true);
        }

        if (error) {
          toast.open({ mesage: error });
        }
      }
    }

    return {
      form,
      v$,
      t,
      handleCheckEmail,
    };
  },
});
</script>
<style lang="scss" scoped>
form.form {
  padding-top: 130px;
}
@media screen and (max-width: 1060px) {
  form.form {
    padding-top: 0px;
  }
}
</style>
