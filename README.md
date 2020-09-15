# Generously Interfacing Named Entities

## Installation

install dependencies

    npm install

serve development version

    npm run serve

build production version to `/dist` directory

    npm run build

then serve `/dist`, e.g. using [`serve`][serve]

    serve -s dist

## Stack

The project uses the following tools:

- [Vue.js](https://vuejs.org/); modulair & reactive front-end framework
- [Tachyons](http://tachyons.io/); atomic styling system

## Component Architecture

The project is structured by the following nested components:

- `App`
  - `VideoComposition`
    - `VideoPlayer`
    - `VideoTranscript`
    - `VideoEntities`

`VideoPlayer`, `VideoTranscript` and `VideoEntities` are sibling components that [emit custom events][emit] - either triggered by user interaction of by video playback - up to their parent component `App`. `App` next communicates back to its child components via [refs][refs].

`App` loads `metadata.json` and passes `layer__asr` and `layer__ner` into its child components as `asrSequences` and `nerSequences` respectively. Child components render UI elements based on these sequences.

## UX considerations

Main goal of the project is to _generously_ display _named entities_ along with their source video in a way that encourages rapid insight in the video content. [Mitchel Whitelaw argues][generous] for _generous_ archival interfaces that:

- are visually rich and browsable
- emphasise exploration and interpretation over task and information retrieval
- reveal the scale and complexity of digital heritage collections (overview)

### App

- Each of the three components in their own way represent content elements of the source video. `VideoPlayer` and `VideoTranscript` are driven by time. `VideoEntities` is 'driven' by entities recognised in the Dutch spoken sound of the video. To encourage the notion of overview the components continuously visualise their synchronised state via a consistent visual style.
- To establish a consistent visual style a simple design system approach is used, relying on:
  - text & background color
  - font & font size
- Recognised entities and transcripts are displayed with a monospaced font (words are set _italic_) to hint at the fact they are machine generated. This is also communicated in their titles with an added 🤖 emoji.
- `VideoPlayer` and `VideoTranscript` are time-based representations of the same media (video and text transcript). To enforce this consideration of equality the two components are positioned side by side.
- A brief textual introduction to the interface is provided to encourage the user to start exploring the video content via one of the three interfaces.

### VideoEntities

`VideoEntities` contains entities extracted from the NER layer of the metadata, reformatted to contain references to ASR sequence and start timecode. This component is positioned full-width as the first element to encourage users to grasp overview of the video content and use the entities for navigation.

The component renders a small coloured tag for every time an entity is detected. This reveals not only which entities are detected but also how often they occus. Entities are ordered by the first time they occur and thus also embed a time-based perspective on the video content. Every entity can be used to navigate to its originating transcript sequence.

The current only displays `concepts` and leaves out the more sparsely represented `persons` and `places`.

### VideoPlayer

Just a regular HTML5 video player with native controls.

### VideoTranscript

This component renders a list of ASR sequences, along with their start timecode and detected entities. Every sequence is clickable to navigate to the respective start time in the video. This kind of time-based navigation is kept at sequence level (rather than word level) to keep the context of the sentence present.

Entities are displayed in a column on the right side. Ideally they would be highlighted in the sequence text. Because the NER metadata doesn't contain a one to one mapping between entities and originating words, this is currently not implemented.

## User testing

The design of the interface would benefit tremendously from additional user testing and evaluation. With the goal of rapid insight in video content in mind several evaluation strategies could be considered:

- Qualitative evalation of the UX via interviews with users.
- Quantitative evaluation of a comprehension of the video content by comparing this interface vs others and testing:
  - recall of the content / topics / entities after using the interface.
  - time required to find topics (either of interest to the user or specified by the experiment).

<!-- References -->

[serve]: https://github.com/zeit/serve "Static file serving and directory listing"
[emit]: https://vuejs.org/v2/guide/components-custom-events.html "Custom events in Vue.js"
[refs]: https://vuejs.org/v2/guide/components-edge-cases.html#Accessing-Child-Component-Instances-amp-Child-Elements "Accessing Child Components via Refs in Vue.js"
[generous]: http://www.digitalhumanities.org/dhq/vol/9/1/000205/000205.html "Mitchell Whitelaw - Generous Interfaces for Digital Cultural Collections"
