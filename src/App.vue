<template>
  <div id="app" class="ma3">
    <h1>Generously Interfacing Named Entities Recognised from Video</h1>
    <p>Philo van Kemenade, October 2019</p>

    <p class="f4">
      Explore video content below by either:
      <ul>
        <li>clicking on any of the <strong>named entities</strong></li>
        <li>using the controls of the <strong>video player</strong></li>
        <li>clicking on a sentence in the <strong>video transcript</strong></li>
      </ul>
    </p>
    <VideoComposition
      :videoSrc="videoSrc"
      :nerSequences="nerSequences"
      :asrSequences="asrSequences"
    />
  </div>
</template>

<script>
import VideoComposition from "./components/VideoComposition";
import metadata from "./assets/metadata-ob.json";

export default {
  name: "App",
  components: {
    VideoComposition,
  },
  data: function() {
    return {
      asrSequences: metadata._source.layer__asr,
      nerSequences: metadata._source.layer__ner,
      videoSrc: metadata._source['@graph']['dcterms:hasFormat'].filter(format => format.endsWith("mp4"))[0]
    };
  },
};
</script>

<style>
:root {
  --bg-color: white;
  --text-color: #2c3e50;
  --active-bg-color: yellow;
  --active-text-color: black;
  --highlight-bg-color: rgba(255, 255, 0, 0.2);
  --entity-concept-bg-color: #CDECFF;
  --entity-concept-text-color: #00449E;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--bg-color);
  color: var(--text-color);
}

@import url("https://fonts.googleapis.com/css?family=Source+Code+Pro:400,400i&display=swap");
.font-mono {
  font-family: "Source Code Pro", monospace;
}

.entity {
  display: inline-block;
  margin: 0.1rem !important;
  padding: 0.1rem;
  font-size: 75%;
  cursor: pointer;
}
.entity.concept {
  background-color: var(--entity-concept-bg-color);
  color: var(--entity-concept-text-color);
}
</style>
