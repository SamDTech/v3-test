<template>
  <nav>
    <div class="nav-group">
      <div class="sidebar-logos">
        <div class="jd-logo-container">
          <img
            v-lazy="{ src: '/images/mini-logo-white.png' }"
            alt="JD icon"
            width="48"
            height="48"
          >
        </div>
        <img
          v-lazy="{ src: '/images/logo.png' }"
          alt="logo Juridoc"
          width="180"
          class="main-logo"
        >
      </div>
      <p>Current plan: Professional</p>
    </div>
    <div class="nav-group">
      <div id="navbar-button">

      </div>
      <!-- <base-button
        v-if="routeName === 'Documents'"
        dashboard
        neutral
      >
        <font-awesome-icon icon="file-alt" />Novo documento
      </base-button>
      <base-button
        v-else-if="routeName === 'Team'"
        dashboard
        neutral
        @click="openInviteModal"
      >
        <font-awesome-icon icon="user-plus" />Novo convite
      </base-button> -->
      <font-awesome-icon icon="bell" />
      <font-awesome-icon icon="graduation-cap" />
      <navbar-profile />
    </div>
  </nav>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import { useRoute } from 'vue-router';
import NavbarProfile from '@/layouts/System/Navbar/NavbarMenu.vue';
import useModal from '@/hooks/useModal';

export default defineComponent({
  components: { NavbarProfile },
  setup() {
    const route = useRoute();
    const routeName = computed(() => route.name);
    function openInviteModal() {
      const modal = useModal();
      modal.open({ component: 'InviteModal' });
    }
    return { routeName, openInviteModal };
  },
});
</script>

<style lang="scss" scoped>
nav {
  display: flex;
  background: $neutral-1;
  justify-content: space-between;
}
.sidebar-logos {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1rem;
  .jd-logo-container {
    display: flex;
    justify-content: center;
    background: $secondary;
    padding: 1.2rem;
    width: 4.5rem;
  }
}
.nav-group {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}
</style>
