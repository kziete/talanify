<template>
  <transition name="fade">
    <div v-if="active" class="wrapper">
      <div :class="[{active: 'hidden'}, 'talana-overlay']" @click="closePanel"></div>
      <transition name="slide-fade">
        <div v-if="activePanel" :class="['talana-panel', {'big': big}]">
          <div class="talana-panel-header">
            <div class="title">
              <slot name="header">
                <h2 class="talana-h2">¡Esto es un panel!</h2>
                <p>Que puede tener una descripción super fabulosa</p>
              </slot>
            </div>
            <div class="actions">
              <v-btn @click="closePanel" flat icon>
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </div>
          </div>
          <div v-if="tabs || searchData" class="panel-controller">
            <ul class="talana-panel-tabs">
              <li v-for="(tab, index) in tabsIds"
                  :class="[{active : activeTab === index}, {'origin-left' : origin === 'left'}, {'origin-right' : origin === 'right'}]"
                  @click="handlePanelTab(index)"
                  v-scroll-to="{
                                 el: `#${tab.hash}`,
                                 container: '#panelBody',
                                 duration: 50,
                                 easing: 'linear',
                                 offset: 0,
                                 onStart: onStartScroll,
                                 onDone: onDoneScroll,
                                 force: false,
                                 cancelable: true,
                                 x: false,
                                 y: true
                             }"
              >
                {{tab.name}}
                <span v-if="disableTabs" class="talana-disable-tabs"></span>
              </li>
            </ul>
            <div v-if="searchData" class="talana-panel-searcher">
              <v-btn @click="searcherActive = !searcherActive" flat icon>
                <v-icon>mdi-magnify</v-icon>
              </v-btn>
              <t-card class="talana-searcher-list-wrapper" v-if="searcherActive">
                <div class="input-wrapper">
                  <v-text-field
                    v-model="searchInput"
                    clearable
                    placeholder="Buscar..."
                  ></v-text-field>
                </div>
                <ul v-if="searchInput.length > 2">
                  <li v-if="filteredItems.length > 0"
                      v-for="option in filteredItems"
                      @click="handleCloseSearcher"
                      v-scroll-to="{
                                 el: `#${option.hash}`,
                                 container: '#panelBody',
                                 duration: 50,
                                 easing: 'linear',
                                 offset: 0,
                                 force: true,
                                 cancelable: true,
                                 x: false,
                                 y: true
                             }">
                    {{option.name}}
                  </li>
                  <div v-if="filteredItems.length < 1" class="talana-searcher-list-empty-state">
                    <p>No he conseguido {{searchInput}} :(</p>
                  </div>
                </ul>
              </t-card>
            </div>
          </div>
          <div ref="panelBody" id="panelBody" :class="['talana-panel-body', {'with-controller': tabs || searchData}]">
            <div v-if="isEmpty" class="talana-panel-empty-state">
              <v-icon class="empty-icon">{{emptyState.icon}}</v-icon>
              <p>{{emptyState.text}}</p>
            </div>
            <slot>
              <div class="talana-panel-empty-state">
                <img src="./assets/monsters/tono.png" alt="">
                <p>{{emptyState.text}}</p>
              </div>
            </slot>
          </div>
          <div class="talana-panel-footer">
            <slot name="footer" class="talana-panel-footer-right">
              <v-btn color="primary" @click="closePanel">Ok!</v-btn>
            </slot>
          </div>
          <div class="talana-panel-intercom-space">
            <div class="talana-panel-intercom-space-message">
              ¿Tienes alguna duda? Estamos aqui para ayudarte
            </div>
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script>
  import tCard from '@/talanify/Card'

  function handleBodyScroll (action) {
    if (action === 'open') {
      document.documentElement.style.overflowY = 'hidden'
      document.body.style.overflowY = 'hidden'
    } else {
      document.documentElement.style.overflowY = 'auto'
      document.body.style.overflowY = 'auto'
    }
  }

  export default {
    name: 'Panel',
    props: {
      isEmpty: {
        type: Boolean,
        default: false,
      },
      tabs: {
        type: Boolean
      },
      searchData: {
        type: Boolean
      },
      emptyState: {
        type: Object,
        default: function () {
          return {
            icon: 'mdi-heart-outline',
            text: '¡Hola! Esto es un empty panel, contiene cuatro slots, uno para el header, otro para el cuerpo y otros dos para las acciones del footer, así que rellenala como quieras ;)'
          }
        }
      },
      active: {
        type: Boolean,
        default: false
      },
      big: {
        type: Boolean,
        default: false
      }
    },
    components: {
      tCard
    },
    data () {
      return {
        activePanel: false,
        activeTab: 0,
        origin: 'left',
        disableTabs: false,
        containerIds: [],
        tabsIds: [],
        searcherActive: false,
        searchInput: ''
      }
    },
    computed: {
      filteredItems () {
        return this.containerIds.filter(item => {
          return item.name.toLowerCase().indexOf(this.searchInput.toLowerCase()) > -1
        })
      }
    },
    created () {

    },
    mounted () {
      // From testing, without a brief timeout, it won't work.
    },
    methods: {
      closePanel () {
        this.$emit('close')
        handleBodyScroll('close')
      },
      handlePanelTab (index) {
        let origin = this.activeTab

        if (origin > index) {
          this.origin = 'right'
        } else {
          this.origin = 'left'
        }

        this.activeTab = index
      },
      onStartScroll () {
        this.disableTabs = true
      },
      onDoneScroll () {
        setTimeout(() => this.disableTabs = false, 50)
      },
      getIds () {
        let ids = this.$refs.panelBody.querySelectorAll('[id]')
        ids.forEach((el) => {
          if (el.tagName === 'H4') {
            this.tabsIds.push({
              hash: el.id,
              name: el.id.replace(/-/g, ' ')
            })
          }

          this.containerIds.push({
            hash: el.id,
            name: el.id.replace(/-/g, ' ')
          })
        })
      },
      handleCloseSearcher () {
        setTimeout(() => this.searcherActive = false, 100)
      }
    },
    watch: {
      active: function (nV) {
        let self = this
        if (nV === true) {
          handleBodyScroll('open')
          setTimeout(function () {
            self.activePanel = self.active
          }, 100)
          setTimeout(function () {
            self.containerIds = []
            self.getIds()
          }, 200)
        }
      },
      searchInput: function (nV) {
        if (nV === null) this.searchInput = ''
      }
    }
  }
</script>

<style lang="scss">
  $controller-height: 50px;
  $header-height: 80px;
  $footer-height: 70px;
  $intercom-space-height: 100px;

  .wrapper {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 200;

    .talana-overlay {
      background-color: rgba(0, 0, 0, .4);
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
    }

    .talana-panel {
      position: fixed;
      right: 0;
      top: 0;
      width: 550px;
      height: 100vh;
      background: #fff;
      box-shadow: 0 24px 38px rgba(0, 0, 0, .3);

      &.big {
        width: 800px;
      }

      &:before {
        display: block;
        content: '';
        height: 6px;
        width: 100%;
        background: var(--talana-blue);
      }

      &-header {
        height: $header-height;
        display: flex;
        border-bottom: 1px solid #D8D8D8;

        .actions {
          width: 74px;
          display: flex;
          align-items: center;
          justify-content: center;
        }

        .title {
          width: calc(100% - 74px);
          padding: 16px 24px;
          box-sizing: content-box;
          font-family: var(--font-family) !important;

          h2, h1, h3, h4 {
            margin-top: 0;
            margin-bottom: 6px;
          }

          p {
            font-size: var(--font-size-body);
            color: #777;
          }
        }
      }

      .panel-controller {
        height: $controller-height;
        box-shadow: 0 2px 2px rgba(#000, .1);
        display: flex;
        padding: 0 24px;

        .talana-panel-tabs {
          list-style: none;
          width: calc(100% - 50px);
          padding-left: 0;
          margin-bottom: 0;

          span.talana-disable-tabs {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: white;
            z-index: 8;
            opacity: .2;
          }

          li {
            text-transform: capitalize;
            height: 100%;
            min-width: 80px;
            justify-content: center;
            display: inline-flex;
            align-items: center;
            padding: 0 16px;
            position: relative;
            font-size: var(--font-size-body);
            color: #757575;

            &:hover {
              cursor: pointer;
              color: var(--talana-blue);
            }

            &:before {
              content: '';
              position: absolute;
              bottom: 0;
              height: 2px;
              width: 0;
              background-color: var(--talana-blue);
            }

            &.origin-left:before {
              left: 0;
              right: auto;
            }

            &.origin-right:before {
              right: 0;
              left: auto;
            }

            &:after {
              position: absolute;
              content: '';
              width: 8px;
              height: 8px;
              border: 8px solid transparent;
              border-bottom-color: var(--talana-blue);
              bottom: 2px;
              left: calc(50% - 8px);
              opacity: 0;
            }

            &.active {
              color: var(--talana-blue);
              font-family: 'Ubuntu Medium', sans-serif;

              &:before {
                transition: .5s all ease;
                width: 100%;
              }

              &:after {
                opacity: 1;
                transition: .5s all ease;
              }
            }
          }
        }

        .talana-panel-searcher {
          position: relative;

          .input-wrapper .v-input__slot {
            padding: 0 16px 14px 16px;
          }

          .v-text-field__details {
            display: none;
          }

          .v-input__slot {
            margin-bottom: 0;
          }

          .talana-searcher-list-empty-state {
            padding: 0 24px;
            color: #9e9e9e;

            p {
              margin-bottom: 0;
              text-align: center;
            }
          }

          .talana-searcher-list-wrapper {
            position: absolute;
            top: 100%;
            right: 5px;
            z-index: 999;
            list-style: none;
            width: 300px;

            ul {
              margin: 8px 0 14px 0;
              padding-left: 0;
              max-height: 150px;
              overflow-y: auto;
            }

            li {
              padding: 0 16px;
              line-height: 32px;
              font-size: 14px;
              list-style: none;
              text-transform: capitalize;

              &:hover {
                background-color: #eee;
                cursor: pointer;
              }
            }
          }
        }
      }

      &-body {
        height: calc(100% - (#{$header-height} + #{$footer-height} + #{$intercom-space-height}));
        scroll-behavior: smooth;
        overflow-y: auto;

        &.with-controller {
          height: calc(100% - (#{$header-height} + #{$footer-height} + #{$intercom-space-height} + #{$controller-height}));
        }
      }


      &-footer {
        padding: 16px 25px;
        border-top: 1px solid rgba(#A9A9A9, .4);
        display: flex;
        align-items: center;
        justify-content: flex-end;

        .v-btn {
          margin: 0 8px 0 0;

          &:last-child {
            margin: 0;
          }
        }

        > div {
          display: flex;
          align-items: center;
        }


        &-left {
          justify-content: flex-start;
        }

        &-left {
          justify-content: flex-end;
        }
      }

      &-intercom-space {
        height: 100px;
        position: relative;
        background-color: #E7F6FA;
        border-top: 1px solid #A2D8EA;

        &-message {
          position: absolute;
          right: 92px;
          top: 28px;
          height: 31px;
          padding: 0 16px;
          background-color: var(--talana-blue);
          border-radius: 4px;
          display: flex;
          align-items: center;
          color: white;

          &:after {
            position: absolute;
            content: '';
            top: calc(50% - 2.5px);
            left: 100%;
            border: 5px solid transparent;
            border-left-color: var(--talana-blue);
          }
        }
      }

      &-empty-state {
        height: 100%;
        color: var(--talana-gray-light);
        display: flex;
        align-items: center;
        justify-content: center;
        flex-direction: column;

        .empty-icon {
          font-size: 45px;
          color: var(--talana-gray-lighter);
          margin-bottom: 15px;
        }

        img {
          opacity: .4;
        }

        p {
          color: inherit;
        }
      }
    }
  }

  .fade-enter-active, .fade-leave-active {
    transition: opacity .2s;
  }

  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */
  {
    opacity: 0;
  }

  .slide-fade-enter-active, .slide-fade-leave-active {
    transition: all .2s ease;
  }

  .slide-fade-enter, .slide-fade-leave-to
    /* .slide-fade-leave-active below version 2.1.8 */
  {
    transform: translateX(100%);
    opacity: 0;
  }

  .v-form > .container {
    padding: 24px;
  }

  .v-form > .container > .layout > .flex {
    padding: 8px;
  }

  .v-form > .container > .layout:only-child {
    margin: -8px;
  }

  .v-form > .container > .layout:not(:only-child) {
    margin: auto -8px;
  }
</style>

<style lang="scss">
</style>
