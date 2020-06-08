# r3f boilerplate

<img src="https://img.shields.io/david/fasani/r3f-boilerplate?color=green"> <img src="https://img.shields.io/david/dev/fasani/r3f-boilerplate?color=green"> <img src="https://img.shields.io/github/license/fasani/r3f-boilerplate?color=black">

A lightweight boilerplate for r3f ([react-three-fiber](https://github.com/react-spring/react-three-fiber))

## Features

- [react-refresh](https://github.com/facebook/react/tree/master/packages/react-refresh) - Edit components without losing their state
- [drei](https://github.com/react-spring/drei) - Helpers for react-three-fiber

## Quickstart

1. Clone this repository using `git clone --depth=1 https://github.com/fasani/r3f-boilerplate.git <YOUR_PROJECT_NAME>`
2. Move to the newly created directory: `cd <YOUR_PROJECT_NAME>`
3. Run `npm install` to install dependencies
3. Run `npm start` to see the example app at http://localhost:3000

## Make your first commit

After cloning to start this repository from your own 'Initial commit' you can completely remove the existing git information by doing the following:

1. `rm -rf .git` this will remove the git information for the repository
2. `git init` this will start the repository with no history from the current state

## Build your project

To build your project you can run `npm run build` any modifications made to the `index.html` will be copied over as the base file in the newly created `dist` folder.

# Introduction resources

- [Bringing WebGL to React - Paul Henschel aka @0xca0a at @ReactEurope 2020](https://www.youtube.com/watch?v=YyqBdN71nFs)
- [Animation and 3D in react-three-fiber (with Paul Henschel) — Learn With Jason](https://www.youtube.com/watch?v=1rP3nNY2hTo)
- [Scroll, Refraction and Shader Effects in Three.js and React](https://tympanus.net/codrops/2019/12/16/scroll-refraction-and-shader-effects-in-three-js-and-react/)
- [Write three.js in React Using react-three-fiber](https://www.digitalocean.com/community/tutorials/react-react-with-threejs)

## Upgrade options

I have kept this repository as lightweight as possible. You could also remove the `drei` dependency if you wish. Drei has many useful helpers for `react-three-fiber` so you should check it out first.

Because the aim is to keep this boilerplate small, you may need to add additional dependencies depending on what you are building I maintain a list of upgrade options below.

### GLTF Loader

If you need to work with GLTF models you will need to do the following:

`npm install gltf-webpack-loader --save-dev`

Modify `webpack.config.js` and add the following to the module.rules array:

```
module: {
    rules: [
        ...
        {
            test: /\.(gltf)$/,
            use: 'gltf-webpack-loader'
        }
    ]
}
```

#### A GLTF model example

```
import React, { Suspense } from 'react';
import { useLoader } from 'react-three-fiber';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import logo from '../assets/models/logo.gltf';

function Logo(props) {
    const gltf = useLoader(GLTFLoader, logo);

    return (
        <group {...props}>
            <scene>
                <mesh castShadow receiveShadow>
                    <bufferGeometry attach="geometry" {...gltf.__$[1].geometry} />
                    <meshPhongMaterial attach="material" color="black" />
                </mesh>
            </scene>
        </group>
    )
}

export default function SuspendedLogo(props) {
    return (
        <Suspense fallback={null}>
            <Logo {...props} />
        </Suspense>
    )
}
```

### Font loader

If you need to work with font blob files you will need to do the following:

`npm install url-loader --save-dev`

Modify `webpack.config.js` and add the following to the module.rules array:

```
module: {
    rules: [
        ...
        {
            test: /\.(blob)$/,
            use: 'url-loader'
        }
    ]
},
```

#### Font loader example

```
import * as THREE from 'three'
import React, { Suspense } from 'react'
import { useLoader } from 'react-three-fiber'
import Font from '../assets/font.blob'

function Text(props) {
    const font = useLoader(THREE.FontLoader, Font);
    const config = {
        font,
        size: 40,
        ...
    }

    return (
        <group {...props}>
            <mesh castShadow position={[-120, 0, 0]}>
                <textGeometry attach="geometry" args={['Test', config]} />
                <meshToonMaterial attach="material" color="black" />
            </mesh>
        </group>
    )
}

export default function SuspendedText(props) {
    return (
        <Suspense fallback={null}>
            <Text {...props} />
        </Suspense>
    )
}
```

### async/await

If you want to fetch data using async/await you will experience a 'regeneratorRuntime is not defined' error which can be fixed by doing the following:

`npm install @babel/plugin-transform-runtime --save-dev`

Modify `.babelrc` and add the following to the plugins section:
```
{
  "presets": [
    "@babel/env",
    "@babel/react"
  ],
  "plugins": [
    "@babel/plugin-transform-runtime"
  ]
}
