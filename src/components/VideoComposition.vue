<template>
  <div>
    <div>
      <VideoEntities
        v-if="nerSequences.length"
        class="ma2"
        ref="videoEntities"
        v-bind:nerSequences="this.nerSequences"
        v-on:jumpToTimecode="onJumpToTimecode"
      />
    </div>
    <div class="flex flex-column flex-row-ns">
      <VideoPlayer
        class="ma2"
        ref="videoPlayer"
        v-bind:videoSrc="this.videoSrc"
        v-on:timeUpdate="onTimeUpdate"
      />
      <VideoTranscript
        class="ma2"
        ref="videoTranscript"
        v-on:jumpToTimecode="onJumpToTimecode"
        v-bind:asrSequences="this.asrSequences"
        v-bind:nerSequences="this.nerSequences"
      />
    </div>
  </div>
</template>

<script>
import VideoPlayer from "./VideoPlayer";
import VideoTranscript from "./VideoTranscript";
import VideoEntities from "./VideoEntities";

export default {
  name: "VideoComposition",
  components: { VideoPlayer, VideoTranscript, VideoEntities },
  data: function() {
    return {
      currentTime: 0
    };
  },
  props: {
    videoSrc: {
      type: String
    },
    asrSequences: {
      type: Array,
      default: () => [],
    },
    nerSequences: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    onJumpToTimecode(tc) {
      // set videoPlayer CurrentTime to Timecode tc in seconds
      this.$refs.videoPlayer.setCurrentTime(tc);
    },
    onTimeUpdate(tc) {
      this.$refs.videoTranscript.setCurrentTime(tc);
      if (this.nerSequences.length) {
        this.$refs.videoEntities.setCurrentTime(tc);
      }
    }
  }
};
</script>