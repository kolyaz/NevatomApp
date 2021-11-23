<template>
    <div v-if="name">
        <item-link-button
        :link="link"
        :name="name"
        />
    </div>
    <div>
    </div>
</template>

<script>
import ItemLinkButton from './ItemLinkButton.vue';

export default {
  data() {
    return {
      name: 'this.itemConf.service.name',
      link: 'this.itemConf.service.ipv4Addresses[0]',
    };
  },
  methods: {
    GenLinks() {
      try {
        if (this.itemConf.service) {
        //   this.name = this.itemConf.service.name;
        //   this.link = this.itemConf.service.ipv4Addresses[0];
        }
        return true;
      } catch (error) {
        return false;
      }
    },
  },
  mounted() {
  },
  components: { ItemLinkButton },
  props: {
    itemConf: Object,
  },

};
</script>

<style >

</style>
