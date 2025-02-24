<template>
  <form
    class="form"
    @submit.prevent
  >
    <div class="form-content">
      <div class="inputs">
        <base-input
          v-model="v$.email.$model"
          :error="v$.email.$errors[0]?.$message"
          :label="t(`ForgotPassword.form.email`)"
          :placeholder="t(`ForgotPassword.form.emailPlaceholder`)"
        />
      </div>
      <div class="buttons">
        <div>
          <base-button
            :disabled="v$.$invalid"
            block
            primary
            @click="handleForgotPassword"
          >
            {{ t(`ForgotPassword.form.sendMailButton`) }}
          </base-button>
        </div>
      </div>
    </div>
  </form>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';
import { useI18n } from 'vue-i18n';
import { useVuelidate } from '@vuelidate/core';
import { useStore } from 'vuex';
import { useRouter } from 'vue-router';
import { required, email } from '@/utils/I18nValidators';
import { RulesType } from '@/types/Vuelidate';
import { ForgotPasswordType } from '@/store/modules/auth';
import useToast from '@/hooks/useToast';

export default defineComponent({
  setup() {
    const { t } = useI18n();
    const store = useStore();
    const router = useRouter();
    const toast = useToast();

    const form = computed<ForgotPasswordType>(() => store.getters['auth/getForgotPasswordData']);

    const rules = {
      email: { required, email },
    } as RulesType;

    const v$ = useVuelidate(rules, form.value);

    async function handleForgotPassword() {
      const isValidate = await v$.value.$validate();

      if (isValidate) {
        form.value.language = window.localStorage.getItem('lang') || 'en-US';

        try {
          const response = await store.dispatch('auth/submitForgotPassword', form);

          if (response.data) {
            router.push({ name: 'Login' });
            store.dispatch('auth/resetForgotPasswordData');
          }
        } catch (err: any) {
          toast.open({ mesage: err?.response?.data?.message });
        }

        router.push({ name: 'Login' });
      }
    }

    return { v$, t, handleForgotPassword };
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
