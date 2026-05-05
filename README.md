# Lo-Fi Mixer

Absolute silence is terrible for focus. I built this because standard white noise apps never quite have the exact mix I want. It is a lightweight client-side Vue dashboard that lets you blend different background sounds to create a custom audio environment for coding or writing.

## Features

- Layer rain, a campfire, and a coffee shop at the same time.
- Every track has its own volume slider, plus a master volume control.
- You are not limited to the default sounds. You can drag and drop your own `.mp3` or `.wav` files directly into the app.
- It defaults to dark mode because staring at a bright white screen while trying to relax is counterproductive.
- Custom audio is handled locally via object URLs and browser storage. There is no backend and your files never leave your machine.

## Tech Stack

I kept the stack fairly light. It runs on Vue 3 using the Composition API and Tailwind CSS for styling. For managing the custom audio uploads and persistence, it relies on LocalStorage and IndexedDB via localforage, built around the native HTML5 Audio API.

## Running it locally

You will need Node.js installed. The standard Vue startup routine applies here.

```bash
git clone https://github.com/NovaNoctua/lofi-mixer.git
cd lofi-mixer
npm install
npm run dev
```

Once the server starts, open `http://localhost:5173` in your browser.

## Usage

Click a sound card to start playing a track, and use the slider beneath it to adjust the volume.

To add your own audio, click the "Add Custom Track" card. You can upload an image and an audio file, and it will save to your browser for future sessions.

## Contributing

If you want to add a feature or fix a bug, feel free to open a pull request. The process is standard: fork the project, create a feature branch, commit your changes, and push.
