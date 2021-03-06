<template>
  <!-- <div>
    <p><button @click="doSomething()">C</button></p>
    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('C')"/>
    <q-btn round color="black" icon="my_location" @click="pressedKey('CD')"/>
    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('D')"/>
    <q-btn round color="black" icon="my_location" @click="pressedKey('DE')"/>
    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('E')"/>

    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('F')"/>
    <q-btn round color="black" icon="my_location" @click="pressedKey('FG')"/>
    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('G')"/>
    <q-btn round color="black" icon="my_location" @click="pressedKey('GA')"/>
    <q-btn round color="white" text-color="black" icon="my_location" @click="pressedKey('A')"/>
    <q-btn round color="black" icon="my_location" @click="pressedKey('B')"/>
  </div> -->
  <div class="q-pa-md q-gutter-sm">
    <!-- <p><button @click="test()">Test</button></p> -->

    <!-- <div v-for="note in twelveToneNotes" :key="note.id">
      <q-btn round :color=note.background_color :text-color=note.foreground_color icon="my_location" @click="pressedKey(note.name)"/>
    </div> -->
    <!-- <div v-for="key in keyboardPitches" :key="key.id"> -->
    <template v-for="key in keyboardPitches">
      <!-- <q-btn round :color=(key.pitch.alter === 0 ? 'white' : 'black') :text-color=(key.pitch.alter === 0 ? 'black' : 'white') icon="my_location" @click="pressedKey(str.concat(key.pitch.step + (key.pitch.alter === 0 ? '' : 'sharp')))"/> -->
      <!-- <q-btn round :color="key.pitch.alter === 0 ? 'white' : 'black'" :text-color="key.pitch.alter === 0 ? 'black' : 'white'" icon="my_location" @click="pressedKey(key.pitch.step)"/> -->

      <!-- <q-btn round :color="key.pitch.alter === 0 ? 'white' : 'black'" :text-color="key.pitch.alter === 0 ? 'black' : 'white'" icon="my_location" @click="pressedKey(key.pitch.step + (key.pitch.alter === 0 ? '' : '#'))"/> -->

      <!-- <q-btn round :color="key.pitch.alter === 0 ? 'white' : 'black'" :text-color="key.pitch.alter === 0 ? 'black' : 'white'" icon="my_location" @click="pressedKey(key)"/> -->

      <q-btn round :color="key.pitch.alter === 0 ? 'white' : 'black'" :text-color="key.pitch.alter === 0 ? 'black' : 'white'" @click="pressedKey(key)" :key="key.id">
        <div class="text-center">
          {{key.pitch.step + key.pitch.octave + (key.pitch.alter === 0 ? '' : '#')}}
        </div>
      </q-btn>
    </template>
  </div>
</template>

<script lang="ts">
import {
  defineComponent, PropType, computed, ref, toRef, Ref
} from '@vue/composition-api'
import { METHODS } from 'http'

// function pressedKey(key: string) {
//   console.log("Pressed: ", key)
// }
function pressedKey(keyboardPitch: KeyboardPitch) {
  console.log("Pressed: ", keyboardPitch.pitch.step + (keyboardPitch.pitch.alter === 0 ? '' : '#'))
  playPitch(keyboardPitch.frequency)

}

function playPitch(frequency: number) {
  var a=new AudioContext()
  var v=a.createOscillator()
  var u=a.createGain()
  v.connect(u)
  v.frequency.value=frequency
  v.type="sine"
  u.connect(a.destination)
  // u.gain.value=100*0.01
  u.gain.value=20*0.01
  v.start(a.currentTime)
  v.stop(a.currentTime+500*0.001)
}

function pressedAKey() {
  var a=new AudioContext()
  var v=a.createOscillator()
  var u=a.createGain()
  v.connect(u)
  v.frequency.value=440
  v.type="sine"
  u.connect(a.destination)
  u.gain.value=100*0.01
  v.start(a.currentTime)
  v.stop(a.currentTime+500*0.001)
}

function useDoSomething () {
  function doSomething () {
    pressedAKey()
  }
  return { doSomething }
}

// MusicXML Ees
// <note>
//   <pitch>
//     <step>E</step>
//     <alter>-1</alter>
//     <octave>4</octave>
//   </pitch>
//   <type>eighth</type>
// </note>

enum PitchStep {
  C = "C",
  D = "D",
  E = "E",
  F = "F",
  G = "G",
  A = "A",
  B = "B",
}

interface Pitch {
  step: PitchStep
  alter: number
  octave: number
}

interface KeyboardPitch {
  pitch: Pitch
  frequency: number
}

// class KeyboardPitch {
//   pitch: Pitch
//   frequency: number
//   constructor(pitch: Pitch, frequency: number) {
//     this.pitch = pitch
//     this.frequency = frequency
//   }
// }

function createTwelvetoneKeyboardPitches () {
  let keyboardPitches: KeyboardPitch[] = []
  let orderedNormalPitches: Pitch[] = [
    {step: PitchStep.C, alter: 0, octave: 4 },
    {step: PitchStep.C, alter: 1, octave: 4 },
    {step: PitchStep.D, alter: 0, octave: 4 },
    {step: PitchStep.D, alter: 1, octave: 4 },
    {step: PitchStep.E, alter: 0, octave: 4 },
    {step: PitchStep.F, alter: 0, octave: 4 },
    {step: PitchStep.F, alter: 1, octave: 4 },
    {step: PitchStep.G, alter: 0, octave: 4 },
    {step: PitchStep.G, alter: 1, octave: 4 },
    {step: PitchStep.A, alter: 0, octave: 4 },
    {step: PitchStep.A, alter: 1, octave: 4 },
    {step: PitchStep.B, alter: 0, octave: 4 }]

  let referenzPitchKey = 9
  let referenzPitchOctave = 4
  let referencePitchFrequency = 440

  let keysInOctave = orderedNormalPitches.length
  let octaves = 9


  // r = 12-th-root-of(2) = 2^(1/12)
  // f_B4 = r^2 * f_A4
  // f_+2 = r^(2) * f_0
  // f_-3 = r^(1/3) * f_0

  console.log("createTwelvetoneKeyboardPitches:")

  for (let octave of Array(octaves).keys()) {
    for (let key of Array(keysInOctave).keys()) {
      // console.log("Octave", octave, "Key", key)
      let differenceKey = key - referenzPitchKey
      let differenceOctave = octave - referenzPitchOctave
      let differenceInPitches = keysInOctave * differenceOctave + differenceKey

      let frequency = 0
      let pitchDistanceBase = Math.pow(2, 1/keysInOctave)

      // if (differenceInPitches < 0) {
      //   frequency = Math.pow(pitchDistanceBase, 1/(-differenceInPitches)) * referencePitchFrequency
      // } else {
      //   frequency = Math.pow(pitchDistanceBase, differenceInPitches) * referencePitchFrequency
      // }
      frequency = Math.pow(pitchDistanceBase, differenceInPitches) * referencePitchFrequency
      // console.log("Frequency", frequency, "DiffInPitches", differenceInPitches, "r", pitchDistanceBase, keysInOctave)
      // keyboardPitches.push(new KeyboardPitch(pitch: {step: orderedNormalPitches[key].step, alter: orderedNormalPitches[key].alter, octave: octave}, frequency: frequency))

      let keyboardPitch: KeyboardPitch = {pitch: {step: orderedNormalPitches[key].step, alter: orderedNormalPitches[key].alter, octave: octave}, frequency: frequency}
      keyboardPitches.push(keyboardPitch)

      // keyboardPitches.push({orderedNormalPitches[key].step, orderedNormalPitches[key].alter, octave}, frequency)
    }
  }


  return keyboardPitches
}

export default defineComponent({
  name: 'NoteKeyboard',
  data() {
    return {
      // noteAttributes: {

      // },
      // notes: [
      //   { 
      //     name: 'C',
      //     background_color: 'white',
      //     foreground_color: 'black',
      //     base_frequency: 123,
      //   }
      // ],
      twelveToneNotes: [
        {
          id: 1,
          name: 'C',
          background_color: 'white',
          foreground_color: 'black',
          frequency: 264
        },
        {
          id: 2,
          name: 'Cis/Des',
          background_color: 'black',
          foreground_color: 'white',
          frequency: 69
        },
        {
          id: 3,
          name: 'D',
          background_color: 'white',
          foreground_color: 'black',
          frequency: 297,
        }
      ],
      // pitches: [

      // ]
      keyboardPitches: createTwelvetoneKeyboardPitches()
    }
  },
  methods: {
    test() {
      createTwelvetoneKeyboardPitches()
    }
  },
  setup () {
    return { pressedKey, ...useDoSomething() }
  }
})
</script>
