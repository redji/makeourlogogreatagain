<template>
  <q-page class="flex">
    <div class="game">
      <div class="tasks">
        <draggable v-bind="dragOptions" v-model="positions" :move="onMove" @start="isDragging=true" @end="isDragging=false" style="height:60%;">
           <transition-group type="transition" name="flip-list" tag="div" class="pickup-cards">
              <div :class="'card pos-' + position" v-for="position in positions"  :key="position">
                <div class="card-face front"></div>
                <div class="card-face back">
                  <img :src="'/statics/0' + position + '.png'" alt="Don't peak!">
                </div>
              </div>
           </transition-group>
        </draggable>
        <draggable class="logo" v-bind="dragOptions" v-model="logoPieces">
          <transition-group type="transition" name="logo" tag="div" >
            <div class="card logoPiece" v-for="logoPiece in logoPieces" :key="logoPiece">
            </div>
          </transition-group>
        </draggable>
      </div>
      <div class="goals">
        <div class="stats">
          <div class="time-spent">Time spent/score {{timeSpent}}</div>
        </div>
        <div class="pickup-cards"></div>
      </div>
    </div>
  </q-page>
</template>
<script>
import draggable from 'vuedraggable'

export default {
  name: 'PageIndex',
  components: {
    draggable
  },
  mounted () {
    const availablePositions = [1, 2, 3, 4, 5]

    for (let i = 0; i < 5; i++) {
      this.positions.push(availablePositions.splice(Math.floor((Math.random() * availablePositions.length)), 1)[0])
    }

    /* for (var a = 0; a < 5; a++) {
      let card = document.createElement('div'),
        backFace = document.createElement('div'),
        frontFace = document.createElement('div'),
        img = document.createElement('img')

      card.className = 'card pos-' + this.positions[a]
      frontFace.className = 'card-face front'
      backFace.className = 'card-face back'

      img.src = '/statics/0' + (a + 1) + '.png'

      backFace.appendChild(img)
      card.appendChild(frontFace)
      card.appendChild(backFace)

      card.addEventListener('click', this.pickCard)

      document.querySelector('.pickup-cards').appendChild(card)
    } */
  },
  methods: {
    stopTimer () {
      this.timerIsStarted = false
    },
    increment () {
      if (this.timeSpent === 10) {
        this.stopTimer()
      } else {
        this.timeSpent++
        setTimeout(this.increment, 1000)
      }
    },
    startTimer () {
      setTimeout(this.increment, 1000)
    },
    pickCard (evt) {
      // evt.currentTarget.classList.toggle('is-flipped')
      if (this.timerIsStarted === false) {
        this.timerIsStarted = true
        this.startTimer()
      }
    },
    onMove ({ relatedContext, draggedContext }) {
      const relatedElement = relatedContext.element
      const draggedElement = draggedContext.element
      return (
        (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      )
    }
  },
  computed: {
    dragOptions () {
      return {
        animation: 0,
        group: 'description',
        disabled: false,
        ghostClass: 'ghost'
      }
    }
  },
  data () {
    return {
      timeSpent: 0,
      timerIsStarted: false,
      positions: [],
      logoPieces: [],
      isDragging: false
    }
  }
}
</script>
