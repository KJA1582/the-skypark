<template>
  <content_controls_stack :translucent="true" :status_padding="true" :shadowed="true" :scroller_offset="{ bottom: 15 }">
    <template v-slot:nav>
      <h2>Remote</h2>
    </template>
    <template v-slot:content>
      <div class="p42_settings_remote">
        <div class="helper_edge_padding helper_nav-margin network-list">
          <p>Following interfaces were found on your system. Scan a code to open the Skypad on a remote device</p>
          <div v-for="(value, key) in networks" v-bind:key="key" class="network-list">
            <qrcode-vue :value="`http://${value[0]}:3756`" :size="150" level="H" render-as="svg" />
            <p class="notice">{{key}}: {{value[0]}}:3756</p>
          </div>
        </div>
      </div>
    </template>
  </content_controls_stack>
  
</template>

<script lang="ts">
import Vue from 'vue';
import QrcodeVue from 'qrcode.vue'

export default Vue.extend({
  name: "p42_settings_remote",
  data() {
    return {
      themeAuto: false,
      networks: this.$root.$data.config.networks,
    }
  },
  components: {
    QrcodeVue,
  },
  methods: {
    listenerWs(wsmsg: any) {
      switch(wsmsg.name[0]){
        case 'transponder': {
          switch(wsmsg.name[1]){
            case 'state': {
              break;
            }
          }
          break;
        }
      }
    },
  },
  created() {
    this.$root.$on('ws-in', this.listenerWs);
  },
  beforeDestroy() {
    this.$root.$off('ws-in', this.listenerWs);
  },
});
</script>

<style lang="scss">
@import '../../../../sys/scss/colors.scss';
.p42_settings_remote {
  .network-list {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 5px;

    & > div {
      gap: 0;
    }
  }
}
</style>