<template>
  <div class="q-pa-md q-gutter-sm">
    <template v-for="key in keyboardPitches">
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

  for (let octave of Array(octaves).keys()) {
    for (let key of Array(keysInOctave).keys()) {
      let differenceKey = key - referenzPitchKey
      let differenceOctave = octave - referenzPitchOctave
      let differenceInPitches = keysInOctave * differenceOctave + differenceKey

      let pitchDistanceBase = Math.pow(2, 1/keysInOctave)

      let frequency = Math.pow(pitchDistanceBase, differenceInPitches) * referencePitchFrequency

      let keyboardPitch: KeyboardPitch = {pitch: {step: orderedNormalPitches[key].step, alter: orderedNormalPitches[key].alter, octave: octave}, frequency: frequency}
      keyboardPitches.push(keyboardPitch)
    }
  }
  return keyboardPitches
}

export default defineComponent({
  name: 'NoteKeyboard',
  data() {
    return {
      keyboardPitches: createTwelvetoneKeyboardPitches()
    }
  },
  methods: {
    test() {
      createTwelvetoneKeyboardPitches()
    }
  },
  setup () {
    return { pressedKey, }
  }
})
</script>
