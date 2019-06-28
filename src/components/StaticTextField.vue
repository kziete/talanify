<template>
  <div ref="staticContainer" class="static-text-field">
    <label>{{ $t('Observaciones') }}</label>
    <dot :msg="desc" :line="2" class="desc" @isDot="isDot"></dot>
    <span v-if="moreText && dot" ref="showMore" @click="handleDot(this)" class="show-more">Ver más</span>
    <span v-if="!dot" @click="handleDot(this)" ref="hide" class="show-more">Ocultar</span>
  </div>
</template>

<script>
  import dot from 'vue-text-dot'

  function removeTooltip () {
    document.querySelector(".talana-tooltip") ? document.querySelector(".talana-tooltip").remove() : '';
  }

  export default {
    name: 'StaticTextField',
    props: {
      origin: {
        type: String,
        default: 'right'
      }
    },
    components: {
      dot
    },
    data () {
      return {
        desc: 'Esto es un texto super largo con el que voy aprobar esta vaina pa poder saber si esto rial sirve porque sino me\n' +
          '      vy a matar ya shao que tal quiero comprobar si esta vaina de verdad sirve porque sino yo como que me voy a morir de pana y to',
        moreText: false,
        dot: true
      }
    },
    created () {
      this.hideOnClickOutside()
    },
    methods: {
      isDot () {
        this.moreText = true
      },
      createTooltip () {
        let tooltipWrap = document.createElement("div"); //creates div
        tooltipWrap.className = 'talana-tooltip'; //adds class
        tooltipWrap.id = 'talana-tooltip'
        tooltipWrap.appendChild(document.createTextNode(this.desc)); //add the text node to the newly created div.

        let firstChild = document.body.firstChild;//gets the first elem after body
        firstChild.parentNode.insertBefore(tooltipWrap, firstChild); //adds tt before elem

        let scrollLeft = window.pageXOffset || document.documentElement.scrollLeft
        let scrollTop = window.pageYOffset || document.documentElement.scrollTop

        let padding = 15
        let linkProps = this.$refs.showMore.getBoundingClientRect();
        let tooltipProps = tooltipWrap.getBoundingClientRect();
        let topPos = (scrollTop + linkProps.top) - ((tooltipProps.height - (tooltipProps.height / 2)) + padding);
        let leftPos = this.origin === 'right' ? (scrollLeft + linkProps.left) - 275 : (scrollLeft + linkProps.left) + 50

        tooltipWrap.setAttribute('style', 'position: absolute; top:' + topPos + 'px;' + 'left:' + leftPos + 'px; ')

        console.log(window.pageYOffset || document.documentElement.scrollTop)
      },
      handleDot (el) {
        if (this.dot) {
          this.dot = false
          this.createTooltip(el)
        } else {
          this.dot = true
          removeTooltip()
        }
      },
      hideOnClickOutside () {
        const scrollListener = event => {
          this.dot = true
          removeTooltip()
          removeClickListener()
        }

        const outsideClickListener = event => {
          console.log('esta vaina')
          console.log(event.target.id)
          if (event.target.id !== 'talana-tooltip' && !this.dot) { // or use: event.target.closest(selector) === null
            if (event.target.className !== 'show-more') {
              console.log('aja aqui toy')
              this.dot = true
              removeTooltip()
              removeClickListener()
            }
          }
        }

        const removeClickListener = () => {
          console.log('verga mami')
          document.removeEventListener('scroll', scrollListener)
          // document.removeEventListener('click', outsideClickListener)
        }

        document.addEventListener('scroll', scrollListener)
        document.addEventListener('click', outsideClickListener)
      }
    }
  }
</script>

<style lang="scss" scoped>
  .static-text-field {
    position: relative;
  }

  .show-more {
    position: absolute;
    right: 0;
    font-size: .8rem;
    color: var(--talana-blue);

    &:hover {
      cursor: pointer;
    }
  }

  .tool {
    position: absolute;

    &.right-origin {
      top: 0;
      max-width: 200px;
      left: 100%;
      background-color: white;
      padding: 10px;
    }
  }
</style>
