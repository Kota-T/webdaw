<template>
  <teleport to="#label_field">
    <VideoTrackLabel
    :trackType="this.trackData.component"
    v-model:name="name"
    v-model:isSelected="isSelected"
    @track-select="select"
    @track-remove="$emit('track-remove')"
    ref="label"
    />
  </teleport>
  <teleport to="#ruler_layer">
    <VideoCanvasContainer
    :videoStream="videoStream"
    @track-select="select"
    ref="container"
    />
  </teleport>
</template>

<script>
import TrackMixin from '../TrackMixin.js';
import VideoTrackLabel from './VideoTrackLabel.vue';
import VideoCanvasContainer from './VideoCanvasContainer.vue';

export default {
  name: 'VideoTrack',
  components: {
    VideoTrackLabel,
    VideoCanvasContainer
  },
  mixins: [TrackMixin],
  props: {
    videoStream: Object
  },
  methods: {
    async getDownloadData(root, index){
      const name = index + "_" + this.name;
      return {
        component: this.trackData.component,
        name: name,
        canvases: await Promise.all(this.$refs.container.getDownloadData(root.folder(name), ".webm"))
      };
    },

    async getUploadData(){
      return {
        id: this.trackData.id,
        component: this.trackData.component,
        name: this.name,
        canvases: await Promise.all(this.$refs.container.getUploadData())
      };
    }
  }
}
</script>
