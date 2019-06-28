<template>
  <div class="talana-card">
    <div v-if="isEmpty" class="talana-card-empty-state">
      <v-icon class="empty-icon">{{emptyState.icon}}</v-icon>
      <p>{{emptyState.text}}</p>
    </div>
    <slot>
      <div class="talana-card-empty-state">
        <img src="./assets/monsters/julls.png" alt="Julls monstruo">
        <p>{{emptyState.text}}</p>
      </div>
    </slot>
  </div>
</template>

<script>
  export default {
    name: 'card',
    props: {
      isEmpty: {
        type: Boolean,
        default: false,
      },
      emptyState: {
        type: Object,
        default: function () {
          return {
            icon: 'mdi-heart-outline',
            text: '¡Hola! Esto es un empty card, contiene un slot, así que rellenala como quieras ;)'
          }
        }
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../assets/scss/variables';

  .talana-card {
    background-color: $white;
    border-radius: $border-radius;
    box-shadow: $box-shadow;
    overflow: hidden;
    margin: 0 0 16px 0;

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
</style>
