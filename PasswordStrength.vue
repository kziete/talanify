<template>
  <div class="password-indicator-wrapper">
    <div class="password-indicator-bar-wrapper">
      <div class="password-indicator-bar" :style="`width: ${indicator}%; background-color: ${barColor}`">
                     <span>
                       {{indicatorLabel}}
                      </span>
      </div>
    </div>
    <div class="password-indicator-label">
      {{indicatorPrompt}}
    </div>
  </div>
</template>

<script>
  export default {
    name: 'passwordStrength',
    props: {
      password: {
        type: String,
        default: ''
      }
    },
    data () {
      return {
        exist: false,
        hasNumber: false,
        hasUppercase: false,
        hasSpecial: false,
        isLongEnough: false,
      }
    },
    computed: {
      indicatorPrompt () {
        if (!this.isLongEnough) return "Vamos, que debes escribir al menos 6 caracteres";
        if (!this.hasUppercase)
          return "Sería fabuloso si añadieras una mayúscula";
        if (!this.hasNumber) return "¿Y si agregas un número?";
        if (!this.hasSpecial) return "¡Los caracteres especiales hacen que las contraseñas sean super fuertes!";
      },
      indicator () {
        let self = (this.exist + this.isLongEnough) * 50

        if (isNaN(self)) {
          return 0
        } else {
          return self
        }
      },
      indicatorLabel () {
        switch (this.indicator) {
          case this.indicator > 0:
            return 'Meh'
          case 50:
            return 'Meh'
          case 75:
            return 'Buena'
          case 100:
            return '¡Excelente!'
          default:
            return ''
        }
      },
      barColor () {
        switch (this.indicator) {
          case this.indicator > 0:
            return 'var(--talana-red)'
           case 50:
            return 'var(--talana-red)'
          case 75:
            return 'var(--talana-blue)'
          case 100:
            return 'var(--talana-green)'
          default:
            return ''
        }
      }
    },
    methods: {
      checkPassword () {
        this.exist = this.password.length > 0
        this.hasNumber = /\d/.test(this.password);
        this.hasUppercase = /[A-Z]/.test(this.password);
        this.hasSpecial = /[!@#\$%\^\&*\)\(+=._-]/.test(this.password);
        this.isLongEnough = this.password.length > 5
      }
    },
    watch: {
      password: function (nV) {
        this.checkPassword()
      },
      isLongEnough: function (nV) {
        if (nV === true) this.$emit('passPassed', true)
        else this.$emit('passPassed', false)
      }
    }
  }
</script>

<style lang="scss" scoped>
  // Defining variables
  $white: #eff7f6;
  $julls-purple: #5b5f97;
  $julls-green: #bce784;
  $julls-blue: #7ae7c7;
  $julls-white: #ebf5df;
  $julls-black: #090c08;

  .wrapper {
    background-color: $white;
    background-image: url('https://images.pexels.com/photos/1048256/pexels-photo-1048256.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=650&w=940');
    background-size: cover;
    min-height: 100vh;
    display: flex;
    align-items: flex-start;
    justify-content: flex-end;
    flex-direction: column;
    font-family: var(--font-family);
    position: relative;
    padding: 60px;
  }

  .v-card {
    min-width: 450px;
    border-radius: 8px;

    .v-form {
      width: 100%;
    }

    img {
      max-width: 500px;
    }
  }

  // Here I'm modifying the shadow card just cause I want it to be pink.
  /* .elevation-20 {
   box-shadow:
   0 10px 13px -6px rgba($orchid-pink,.5),0 20px 31px 3px rgba($orchid-pink,.6),0 8px 38px 7px rgba($orchid-pink,.5)!important;
  } */

  .wrapper .primary--text {
    color: $julls-black !important;
  }

  // Here's where magic really happens

  .password-indicator {
    &-wrapper {
      width: 100%;
      height: 50px;
      overflow: hidden;
    }

    &-bar {
      transition: 0.5s all ease;
      will-change: width;
      height: 100%;
      background-color: $julls-blue;
      position: absolute;
      border-radius: 30px;

      span {
        position: absolute;
        right: 4px;
        color: #fff;
      }

      &-wrapper {
        background: $white;
        height: 18px;
        width: 100%;
        border-radius: 30px;
        position: relative;
      }
    }
  }

  .credits {
    margin-top: 1rem;
    color: $white;
    position: relative;
    z-index: 9999;

    .avy {
      max-width: 30px;
      border-radius: 50%;
      margin-left: 10px;
      vertical-align: middle;
      cursor: pointer;
    }
  }

</style>
