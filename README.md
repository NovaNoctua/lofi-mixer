# Lo-Fi Mixer

I built this because absolute silence is distracting. This is a Vue dashboard that lets you mix different background sounds to create a custom audio environment for coding, writing, or just staring at the wall.

## The setup

It is a basic grid interface. You click an icon to start a track, and use the slider underneath to adjust its volume. You can layer as many sounds as you want.

I kept the stack as light as possible. It is just Vue. There is no backend, no database, and no user accounts. You load the page and it works.

## Features

- **Layered audio:** Play rain, a crackling campfire, and a moving train simultaneously.
- **Granular volume control:** Every track has its own volume slider bound via `v-model`.
- **Dark mode:** Included by default, because staring at a bright white screen while trying to relax defeats the purpose.
- **Static deployment:** The whole thing builds to static files. You can host it anywhere.

## Running it locally

The standard Vue routine applies here. Clone the repository, install the dependencies, and start the development server.

```bash
git clone https://github.com/yourusername/lofi-mixer.git
cd lofi-mixer
npm install
npm run dev
```

## A note on the audio files

I did not include the actual `.mp3` files in this repository to avoid licensing headaches. To make it work on your machine, you need to add your own audio files.

Create an `audio` folder inside your `public` directory and drop your tracks there. The component logic looks for files named `rain.mp3`, `campfire.mp3`, and so on. You can easily modify the track names in the main array if you want to use different files.

## What I learned building this

Managing HTML5 audio elements inside a reactive framework is slightly annoying. You have to be careful about how you instantiate the audio objects and make sure you destroy them properly, or you end up with ghost audio playing in the background after you pause the interface. It takes some wrangling to keep the audio contexts clean, but the result is a tool I actually keep open in a pinned tab every day.
