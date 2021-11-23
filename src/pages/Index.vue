<template>
  <q-page class=" main">
         <div class="flex flex-center">
          <q-btn
            class="btnMain"
            color="primary"
            label="Устройство в текущей сети"
            icon-right="search"
            @click="GetItems">
            <Spinner v-if="isLoadItems"/>
          </q-btn>
          <q-btn
          class="btnMain"
          color="primary"
          label="Переход в NevatomCloud"
          icon-right="cloud_upload"
           @click="GetCloud" />
         </div>
          <div v-if="btnItem.length">
            <h5  class="flex flex-center ">Устройство найдено
            </h5>
            <ItemLinkButton
            v-for="(item, index) in btnItem"
            :key= "item.service.name"
            :name="item.service.name"
            :link="item.service.ipv4Addresses[0]"
            :number="index+1"
            />
          </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
// import { openURL } from 'quasar';
import Spinner from 'src/components/Spinner/Spinner.vue';
import ItemLinkButton from 'src/components/ItemsLinkButton/ItemLinkButton.vue';

export default defineComponent({
  components: {
    Spinner,
    ItemLinkButton,
  },
  name: 'PageIndex',
  data() {
    return {
      itemConf: {},
      btnItem: [],
      link: '',
      currentName: 'Nevatom',
      isLoadItems: false,
      i: 0,
    };
  },
  methods: {
    GetItems() {
      const { zeroconf } = cordova.plugins;
      this.itemConf = '';
      this.isLoadItems = true;
      this.btnItem = [];
      try {
        zeroconf.close();
        zeroconf.watch('_http._tcp.', 'local.', (result) => {
          if (result) {
            if (result.service.name.indexOf(this.currentName) >= 0) {
              this.isLoadItems = false;
              this.itemConf = { ...this.itemConf, ...result };
              this.i += 1;
            }
          } else {
            // this.itemConf = '';
          }
        });
        return true;
      } catch (error) {
        return false;
      }
    },
    GetCloud() {
      const links = cordova.InAppBrowser.open('https://nevatom-cloud.ru', '_blank', 'location=no');
      links.show();
    },
  },
  watch: {
    itemConf(newItem, oldItem) {
      if (newItem && newItem.service.ipv4Addresses[0]) {
        if (newItem.service.ipv4Addresses[0] !== oldItem.service.ipv4Addresses[0]) {
          this.btnItem = [...this.btnItem, newItem];
        }
      }
    },
    // currentTime(time) {
    //   if (time === 0) {
    //     this.StopTimer();
    //   }
    // },
  },
  mounted() {

  },
});
</script>
<style scoped>
.btnMain{
  margin-top: 30px;
  min-width: 300px;
  min-height: 70px;
}
</style>
