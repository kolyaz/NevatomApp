<template>
    <div class="flex flex-center">
        <q-btn
        class="btnItem"
        label=""
        icon-right="east"
        @click="GetItem">
          {{`${number}) ${name} ip:${link}`}}
        </q-btn>
    </div>
</template>

<script>
export default {
  data() {
    return {

    };
  },
  methods: {
    GetItem() {
      const links = cordova.InAppBrowser.open(`http://${this.link}:3001`, '_blank', 'location=no');
      links.show();
    },
  },
  props: {
    link: {
      type: String,
    },
    name: {
      type: String,
    },
    number: {
      type: String,
    },
  },
};
</script>

<style  scoped>
.btnItem{
  color: "grey";
  margin-bottom: 30px;
  min-width: 300px;
  min-height: 50px;
}
</style>
