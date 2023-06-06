<script lang="ts">
	import { Pane } from 'tweakpane'
	import { browser } from '$app/environment'

	import * as Threlte from '@threlte/core'
	import * as Three from 'three'
	import * as Extra from '@threlte/extras'
	import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass'

	import Background from './background.svelte'
	import garden from '$lib/models/garden.glb'
	import ghost1 from '$lib/models/ghost.glb'
	import ghost2 from '$lib/models/ft.glb'

	type Camera = keyof typeof camera
	type PointLight = keyof typeof pointLight
	type Ghost = keyof typeof ghost
	let ft={
		position: { x: -9.33, y: 0, z: 0 },
		scale:4.5,
		rotation:{x:0,y:110,z:0}
	}

	let garden1 = {
		position:{x:0,y:-0.375,z:0},
	}
	
	let camera = {
		position: { x: 10, y: 10, z: 10 },
		zoom: 25,
	}

	let pointLight = {
		position: { x: 0, y: 2, z: 2 },
		color: '#ff0000',
	}

	let ghost = {
		position: { x: 0, y: 1.9, z: 0 },
		scale: 0.4,
	}

	function float() {
		const timeInSeconds = Date.now() / 1000
		const offsetY = 2.9
		ghost.position.y = Math.sin(timeInSeconds) + offsetY
		requestAnimationFrame(float)
	}

	if (browser) {
		float()

		const pane = new Pane({
			// container:document.getElementById("kala"),
			 title: 'Scene' 
			})

		const cameraControls = pane.addFolder({ title: 'Camera' })
		cameraControls.addInput(camera, 'position')
		cameraControls.addInput(camera, 'zoom')

		cameraControls.on('change', ({ presetKey, value }) => {
			camera[presetKey as Camera] = value as any
		})

		const pointLightControls = pane.addFolder({ title: 'Point Light' })
		pointLightControls.addInput(pointLight, 'position')
		pointLightControls.addInput(pointLight, 'color')

		pointLightControls.on('change', ({ presetKey, value }) => {
			pointLight[presetKey as PointLight] = value as any
		})

		const ghostControls = pane.addFolder({ title: 'Ghost' })
		ghostControls.addInput(ghost, 'position')
		ghostControls.addInput(ghost, 'scale')

		ghostControls.on('change', ({ presetKey, value }) => {
			ghost[presetKey as Ghost] = value as any
		})
	}

	const gridHelper = new Three.GridHelper(20, 10)
	const axesHelper = new Three.AxesHelper(10)
</script>
<div id="kala" style="width: 300px;">
</div>
<Threlte.Canvas rendererParameters={{ antialias: true }}>
	<Threlte.Object3DInstance object={gridHelper} />
	<Threlte.Object3DInstance object={axesHelper} />

	<Threlte.Pass
		pass={new UnrealBloomPass(new Three.Vector2(1, 1), 4, 1, 0.7)}
	/>
	<Threlte.DirectionalLight
	color="white"
	intensity={0.1}
	position={{ x: 10, y: 10 }}
	shadow={{
	  camera: { top: 8 },
	}}
  />
	<Background color="orangered" />

	<Threlte.OrthographicCamera {...camera}>
		<Threlte.OrbitControls />
	</Threlte.OrthographicCamera>

	<Threlte.AmbientLight color="blue" intensity={0.5} />

	<Threlte.PointLight intensity={0.2} {...pointLight} />

	<Extra.GLTF url={ghost2} {...ft} />
	<Extra.GLTF url={ghost1} {...ghost} />
	<Extra.GLTF url={garden} {...garden1}/>
</Threlte.Canvas>
