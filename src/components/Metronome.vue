<template>
  <div>
    <p>Metronome</p>
    <q-btn round color="blue" icon="time" @click="alert('Metronome start/stop...')"/>
  </div>
</template>

<script lang="ts">
import {
  defineComponent, PropType, computed, ref, toRef, Ref
} from '@vue/composition-api'
import { METHODS } from 'http'

// function pressedAKey() {
//   var a=new AudioContext()
//   var v=a.createOscillator()
//   var u=a.createGain()
//   v.connect(u)
//   v.frequency.value=440
//   v.type="sine"
//   u.connect(a.destination)
//   u.gain.value=100*0.01
//   v.start(a.currentTime)
//   v.stop(a.currentTime+500*0.001)
// }

// function useDoSomething () {
//   function doSomething () {
//     pressedAKey()
//   }
//   return { doSomething }
// }

export default defineComponent({
  name: 'NoteKeyboard',
  setup () {
    // return { ...useDoSomething() }
    return {}
  }
})
</script>
