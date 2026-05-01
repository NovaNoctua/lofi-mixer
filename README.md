# Lo-Fi Mixer

I built this because absolute silence is distracting, and standard white noise apps never quite have the exact mix I want. This is a Vue dashboard that lets you mix different background sounds to create a custom audio environment for coding or writing.

## The setup

It is a basic grid interface. You click an icon to start a default track, and use the slider underneath to adjust its volume.

The best part is that you are not limited to the default sounds. You can upload your own `.mp3` or `.wav` files directly into the app. Everything happens locally in your browser. There is no backend, no database, and your files are never uploaded to a server.

## Features

- **Layered audio:** Play rain, a crackling campfire, and a moving train simultaneously.
- **Bring your own noise:** Drag and drop your own audio files to add them to your custom mix.
- **Granular volume control:** Every track has its own volume slider.
- **Dark mode:** Included by default, because staring at a bright white screen while trying to relax defeats the purpose.
- **Privacy first:** Custom audio is handled via local object URLs. Nothing leaves your machine.

## Running it locally

The standard Vue routine applies here. Clone the repository, install the dependencies, and start the development server.

```bash
git clone https://github.com/yourusername/lofi-mixer.git
cd lofi-mixer
npm install
npm run dev
```

## A note on the default audio

I did not include the actual default audio files in this repository to avoid licensing headaches. To make the default grid work on your machine, create an `audio` folder inside your `public` directory and drop your tracks there (e.g., `rain.mp3`, `campfire.mp3`). The custom upload feature, however, will work immediately out of the box.

## What I learned building this

Managing HTML5 audio elements inside a reactive framework is slightly annoying, but adding local file uploads made it much more interesting. I had to learn how to use `URL.createObjectURL()` to turn a user's uploaded file into a playable audio source without a backend. It also forced me to learn about memory management. You have to remember to call `URL.revokeObjectURL()` when a track is removed, or the browser will just quietly leak memory until the tab crashes.
