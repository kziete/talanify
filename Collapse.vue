<template>
  <t-card>
    <div class="talana-collapse">
      <div :class="['talana-collapse-header', {'expanded' : expanded}]"
           @click="expanded = !expanded"
      >
        <div class="talana-collapse-header-indicator-left"></div>
        <div class="talana-collapse-header-indicator-bottom"></div>
        <div class="talana-collapse-header-title">
          <h2 class="talana-h2">{{title}}</h2>
          <p class="description">{{description}}</p>
        </div>
        <div class="talana-collapse-header-chevron">
          <v-icon>mdi-chevron-down</v-icon>
        </div>
      </div>
      <transition name="expand">
        <div v-if="expanded" class="talana-collapse-body">
          <slot>
            <div class="talana-collapse-empty-state">
              <img src="./assets/monsters/sebas.png" alt="Julls monstruo">
              <p>{{emptyState.text}}</p>
            </div>
          </slot>
        </div>
      </transition>
    </div>
  </t-card>
</template>

<script>
  import tCard from './Card'

  export default {
    name: "Collapse",
    components: {
      tCard
    },
    props: {
      title: {
        type: String,
        default: 'Oh mira, soy un collapse'
      },
      description: {
        type: String,
        default: 'Y también puedo tener unas descripciones maravillosas'
      },
      isEmpty: {
        type: Boolean,
        default: false,
      },
      emptyState: {
        type: Object,
        default: function () {
          return {
            icon: 'mdi-heart-outline',
            text: '¡Hola! Esto es un empty collapse, contiene un slot, así que rellenala como quieras ;)'
          }
        }
      }
    },
    data () {
      return {
        expanded: false
      }
    }
  }
</script>

<style lang="scss" scoped>
  .talana-collapse {

    &-header {
      height: 85px;
      position: relative;
      padding: 0 20px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      cursor: pointer;

      &-indicator-left {
        position: absolute;
        bottom: 0;
        left: 0;
        height: 100%;
        transition: .25s all ease;
        width: 2px;
        background: var(--talana-blue);
      }

      &-indicator-bottom {
        position: absolute;
        width: 0;
      }

      &-chevron {
        transition: .4s ease all;
      }

      &.expanded {
        .talana-collapse-header-indicator-left {
          position: absolute;
          height: 0;
        }

        .talana-collapse-header-indicator-bottom {
          bottom: 0;
          left: 0;
          width: 100%;
          transition: .4s all ease;
          height: 2px;
          background: var(--talana-blue);
        }

        .talana-collapse-header-chevron {
          transform: rotate(180deg);
        }
      }
    }

    &-empty-state {
      height: 250px;
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

      p {
        color: inherit;
      }

      img {
        opacity: .4;
      }
    }

  }

  .expand-enter-active, .expand-leave-active {
    max-height: 10000px;
    opacity: 1;
    transition: all ease .35s;
  }

  .expand-enter, .expand-leave-to {
    opacity: 0;
    overflow: hidden;
    max-height: 0;
    transition: all ease .35s;
  }

  .description {
    color: #777;
  }

</style>
