---
title: "Artifact: Three.js Art Gallery"
date: 2023-12-06T14:40:42-05:00
slug: artifact-adventure
type: posts
draft: false
---
<!-- ! Post Header -->

A Three.js 3D virtual reality demo featuring The Met Museum API
<div class="stack-logos">
  <img alt="Javascript" src="https://img.shields.io/badge/-JavaScript-F1DC56?style=flat-square&logo=javascript&logoColor=black" />
  <img alt="Three.js" src="https://img.shields.io/badge/-Three.js-222222?style=flat-square&logo=threedotjs&logoColor=079EF4" />
  <img alt="html5" src="https://img.shields.io/badge/-HTML5-E34F26?style=flat-square&logo=html5&logoColor=white" />
  <img alt="css3" src="https://img.shields.io/badge/-CSS3-264de4?style=flat-square&logo=css3&logoColor=white" />
</div>
<!-- ! "Read more..." -->
<!--more-->

Artifact is a dynamic Three.js demo that transforms curated art from The Met Museum's API into an immersive 3D experience directly in your browser.

[Visit Artifact](https://garysbot.github.io/artifact-adventure/)
<br>
![Artifact Environment GIF](https://github.com/garysbot/artifact-adventure/raw/main/static/readme/gifs/environment.gif)<br><br>

## Features
In Artifact, users:
- Navigate a first-person 3D virtual art exhibit with WASD and mouse controls.
- Render different virtual environments instantly.
- Lighting, shadows, textures, and reflections respond in real-time to their virtual environments.
- View artwork from The Met Museum API rendered in 3D.
<br><br>

## Instructions
Interacting with [Artifact](https://garysbot.github.io/artifact-adventure/) is easy and intuitive:
- ***Start*** the demo with a ***left-click*** on your mouse.
- ***Move*** around the 3D environment with your ***WASD*** keys.
- ***Look*** at your surroundings with your ***mouse***.
- ***Jump*** with the ***spacebar***.
- ***Pause*** the demo by pressing ***ESC***.
<br><br>

## Object-Oriented Design Principles
Each Three.js component handles a specific part of WebGL to render high-performance 3D graphics in your browser.

Artifact is composed of the following components:
- Camera
- Scene
- Lights
- Mesh Objects
- Environment Map
- Renderer
<br><br>

## The Experience
![Artifact Environment GIF](https://github.com/garysbot/artifact-adventure/raw/main/static/readme/gifs/environment.gif)<br>

The `Experience` class manages the complete experience by:
1. Accepting a HTML `canvas` DOM element as an argument,
2. Instantiating all necessary component classes and,
3. Handles persistent animation via a `requestAnimationFrame` loop.

```javascript
// Experience.js
this.debug = new Debug();
this.sizes = new Sizes();
this.time = new Time()
this.scene = new THREE.Scene();
this.camera = new Camera()
this.environment = new Environment();
this.renderer = new Renderer()

this.time.on('tick', () => {
  this.update();
})
```
<br>

![Artifact Animation GIF](https://github.com/garysbot/artifact-adventure/raw/main/static/readme/gifs/animation.gif)<br>

```javascript
// Experience.js
requestAnimationFrame(this.update);

const time = performance.now();
const delta = ( time - prevTime ) / 1000;

velocity.x -= velocity.x * 10.0 * delta;
velocity.z -= velocity.z * 10.0 * delta;

this.camera.controls.moveRight( - velocity.x * delta );
this.camera.controls.moveForward( - velocity.z * delta );

this.renderer.instance.render( this.scene, this.camera );
this.renderer.update();
```
> The `update()` class function listens for WASD key presses to trigger and update camera movement while re-rendering in real-time to simulate movement throughout the 3D environment.<br><br>
> **Note:** Excerpted for length & clarity.
<br><br>

## The Environment
The `Environment` class handles the creation and rendering of the lighting, environment map and mesh objects.
<br>

![Artifact Environment Map GIF](https://github.com/garysbot/artifact-adventure/raw/main/static/readme/gifs/environmentmap.gif)<br>

```javascript
setEnvironmentMap = () => {
  this.environmentMap = this.textureLoader.load('/textures/environmentMap/00.png');
  this.environmentMap.mapping = THREE.EquirectangularReflectionMapping;
  this.environmentMap.colorSpace = THREE.SRGBColorSpace;
  this.scene.background = this.environmentMap;
}
```
<br>

![Artifact Floor GIF](static/readme/gifs/floor.gif)<br>

```javascript
setFloor = () => {
  this.floorGeometry = new THREE.CircleGeometry( 100, 64, 0, 2 * Math.PI );
  this.floorGeometry.rotateX( - Math.PI / 2 );

  const floorTexturePathURL = '/textures/floor/dirt/color.jpg'
  const floorTexture = this.textureLoader.load(floorTexturePathURL);
  floorTexture.repeat.set(20, 20);
  floorTexture.wrapS = THREE.RepeatWrapping;
  floorTexture.wrapT = THREE.RepeatWrapping;

  const floorMaterial = new THREE.MeshBasicMaterial( { map: floorTexture } );
  
  const floor = new THREE.Mesh( this.floorGeometry, floorMaterial );
  this.scene.add(floor);
}
```
<br>

![Artifact Lighting GIF](static/readme/gifs/lighting.gif)<br>

```javascript
setLight(){
  this.light = new THREE.HemisphereLight( 0xeeeeff, 0x777788, 2.5);
  this.light.position.set( 0.5, 1, 0.75 );
  this.scene.fog = new THREE.Fog( 0xffffff, 0, 750 );
  this.scene.add(this.light);
  //...
}
```
<br>

## Tech Stack
- Node.js
- Three.js
- lil-gui
- Vite
- HTML5 & CSS
- Blockade Labs Skybox AI
- Sketchfab
- DALL-E 3
- The Metropolitan Museum of Art Collection API
<br>

## Future
- Resolve disposal of objects in refactored version to reduce lag.
- Add "Teleport" feature to allow user to change all environment features simultaneously.
- Add music for each gallery room with play & pause.
