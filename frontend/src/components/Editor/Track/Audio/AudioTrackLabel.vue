<template>
  <TrackLabel
  :isSelected="isSelected"
  @update:isSelected="v=>$emit('update:isSelected', v)"
  @track-select="shiftKey=>$emit('track-select', shiftKey)"
  @track-remove="$emit('track-remove')"
  ref="container"
  >
    <template v-slot:track-type>{{ trackType.slice(0, -5) }}</template>
    <template v-slot:contents>
      <InputElement type="text" size="8" :modelValue="name" @update:name="v=>$emit('update:name', v)" />
      <div class="track-slider-container">
        <select v-model="selectedSlider">
          <option selected title="音量">V</option>
          <option title="パン">P</option>
        </select>
        <input type="range" min="0" max="1" step="0.05" v-show="selectedSlider==='V'" :value="String(gain)" @input="$emit('update:gain', Number($event.target.value))" :title="'音量:' + gain">
        <input type="range" min="-1" max="1" step="0.1" v-show="selectedSlider==='P'" :value="String(pan)"  @input="$emit('update:pan', Number($event.target.value))"  :title="'パン:' + pan">
      </div>
      <div class="track-btn-container">
        <button type="button" class="track-btn track-monitor-btn" @click="$emit('update:isMonitoring', !isMonitoring);" title="入力モニタリング">
          <svg v-show="!isMonitoring"><use href="../../../../assets/monitor_off.svg#monitor_off" fill="#323232"/></svg>
          <svg v-show="isMonitoring"><use href="../../../../assets/monitor_on.svg#monitor_on" fill="red"/></svg>
        </button>
        <button type="button" class="track-btn track-mute-btn" @click="$emit('update:isMuted', !isMuted);" title="消音">
          <svg v-show="!isMuted"><use href="../../../../assets/volume_on.svg#volume_on" fill="#323232" stroke="#323232"/></svg>
          <svg v-show="isMuted"><use href="../../../../assets/volume_off.svg#volume_off" fill="#323232" stroke="#323232"/></svg>
        </button>
        <button type="button" class="track-btn track-solo-btn" :class="{active: isSolo}" @click="$emit('update:isSolo', !isSolo);" title="ソロ再生">Solo</button>
      </div>
    </template>
  </TrackLabel>
</template>

<style>
.track-slider-container > *{
  vertical-align: middle;
}
.track-slider-container select{
  margin-right:5px;
}
.track-btn-container{
  margin-top: 10px;
}
.track-btn{
  background-color: white;
  width: 35px;
  height: 25px;
  border-radius: 5px;
  margin-right: 10px;
  vertical-align: middle;
}
.track-btn:hover{
  cursor: pointer;
}
.track-btn:last-child{
  margin-right: 0;
}
.track-monitor-btn{
  padding: 3px 0;
}
.track-monitor-btn svg{
  width: 19px;
  height: 19px;
}
.track-mute-btn{
  padding: 0px 5px;
}
.track-mute-btn svg{
  width: 25px;
  height: 25px;
}
.track-solo-btn{
  font-size: 14px;
}
.track-solo-btn.active{
  background-color: gold;
}
</style>

<script>
import TrackLabel from '../TrackLabel.vue';

export default {
  name: 'AudioTrackLabel',
  props: {
    trackType: String,
    name: String,
    gain: Number,
    pan: Number,
    isSelected: Boolean,
    isMonitoring: Boolean,
    isMuted: Boolean,
    isSolo: Boolean
  },
  emits: [
    'update:name',
    'update:gain',
    'update:pan',
    'update:isSelected',
    'update:isMonitoring',
    'update:isMuted',
    'update:isSolo',
    'track-select',
    'track-remove'
  ],
  components: {
    TrackLabel
  },
  data(){
    return {
      selectedSlider: "V"
    }
  }
}
</script>
