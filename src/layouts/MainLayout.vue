<template>
  <q-layout view="hHf LpR fff" >
    <q-header  class="bg-white">
      <q-toolbar >
        <q-btn
          class="bg-grey-5"
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
        <q-toolbar-title>
         <Header/>
        </q-toolbar-title>
      </q-toolbar>

    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      side="left"
    >
      <q-list >
        <q-item-label
          header
        >
        </q-item-label>

        <EssentialLink
          v-for="link in linksList"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>
    <q-page-container  :style=" `width:100vw;`">
      <!-- <Spinner/> -->
      <router-view
      />
    </q-page-container>
  </q-layout>
</template>

<script>
import Header from 'components/Header/Header.vue';
import EssentialLink from 'src/components/Menu/EssentialLink.vue';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'MainLayout',
  data() {
    return {
      isWebsocket: false,
      badge: null,
      linksList: [
        {
          title: 'Главная',
          caption: 'Главная',
          icon: 'house',
          link: '/',
        },
        {
          title: 'Техподдержка',
          caption: 'Техподдержка',
          icon: 'help_center',
          // link: 'settings',
          fun: this.GetHelp,
        },
        {
          title: 'Выход',
          caption: 'Выход',
          icon: 'logout',
          // link: 'trends',
          fun: this.GetExit,
        },
      ],
    };
  },

  components: {
    EssentialLink,
    Header,
  },

  methods: {
    GetExit() {
      navigator.app.exitApp();
    },
    GetHelp() {
      const links = cordova.InAppBrowser.open('https://service.nevatom.ru/', '_blank', 'location=no');
      links.show();
    },
  },
  mounted() {

  },
  setup() {
    const leftDrawerOpen = ref(false);
    return {
      // essentialLinks: this.linksList,
      leftDrawerOpen,
      toggleLeftDrawer() {
        leftDrawerOpen.value = !leftDrawerOpen.value;
      },
    };
  },
});
</script>
