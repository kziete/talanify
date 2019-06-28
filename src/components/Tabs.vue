<template>
  <v-tabs
    show-arrows
  >
    <v-tabs-slider></v-tabs-slider>
    <v-tab
      v-for="(i, index) in items"
      :href="'#tab-' + index"
    >
      {{i.title}}
    </v-tab>

    <v-tabs-items>
      <v-tab-item
        v-for="(i, index) in items"
        :value="'tab-' + index"
      >
        <div class="tabs-content-container" v-if="i.contentType === 'html'" v-html="i.content"></div>
        <div class="tabs-content-container" v-else="">{{i.content}}</div>
      </v-tab-item>
    </v-tabs-items>
  </v-tabs>
</template>

<script>
  export default {
    name: 'Tabs',
    props: {
      items: {
        default: function () {
          return [{
            title: 'Tab 1',
            contentType: 'text',
            content: 'Yo soy texto simple :D'
          },
          {
            title: 'Tab 2',
            contentType: 'html',
            content: '<h1>Contengo HTML!</h1>'
          }]
        },
        type: Array
      }
    },
    data () {
      return {
        text: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.'
      }
    }
  }
</script>

<style lang="scss" scoped>
  .v-tabs__div {
    text-transform: none;
  }

  .v-tabs__slider {
    position: relative;
    background-color: var(--talana-blue);

    &:after {
      content: '';
      display: block;
      width: 16px;
      height: 5px;
      border: 8px solid transparent;
      border-bottom: 5px solid var(--talana-blue);
      position: absolute;
      bottom: 0;
      left: calc(50% - 8px);
    }
  }

  .tabs-content-container {
    padding: 15px;
  }
</style>

<style lang="scss">
  .v-tabs__div {
    a.v-tabs__item--active {
      font-weight: 600;
      color: var(--talana-blue);
    }
  }

  .v-tabs__container {
    border-bottom: 2px solid rgba(#A9A9A9, .4);
  }

  .v-tabs__slider-wrapper {
    bottom: -2px;
  }
</style>
