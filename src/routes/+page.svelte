<script lang="ts">
	import * as Threlte from '@threlte/core'
	import * as Three from 'three'
	import * as Utils from 'three/src/math/MathUtils'
	const gridHelper = new Three.GridHelper(20, 10)
 	const axesHelper = new Three.AxesHelper(10)
	import { Pane } from 'tweakpane'
  	import { browser } from '$app/environment'

	const sphere = {
		position: { x: 0, y: 4, z: 0 },
	}

	if (browser) {
		const pane = new Pane({
			
			 title: 'Scene' ,
		})
		
		const sphereControls = pane.addFolder({ title: 'Sphere' })
		sphereControls.addInput(sphere, 'position')

		sphereControls.on('change', ({ value }) => {
		sphere.position = value as any
		})
	}
  </script>

  <div class="scene">
	<Threlte.Canvas>
	  <!-- Camera -->
	  <Threlte.PerspectiveCamera position={{ x: 20, y: 20, z: 20 }} fov={50}>
		<!-- Controls -->
		<Threlte.OrbitControls autoRotate />
	  </Threlte.PerspectiveCamera>
  
	  <!-- Lights the scene equally -->
	  <Threlte.AmbientLight color="white" intensity={0.2} />
  
	  <!-- Light that casts a shadow -->
	  <Threlte.DirectionalLight
		color="white"
		intensity={2}
		position={{ x: 10, y: 10 }}
		shadow={{
		  camera: { top: 8 },
		}}
	  />
	
  
	  <!-- Sphere -->
	  <Threlte.Mesh
		geometry={new Three.SphereGeometry(4, 64, 64)}
		material={new Three.MeshStandardMaterial({ color: 'white' })}
		position={sphere.position}
		receiveShadow
		castShadow
	  />
  
	  <!-- Floor -->
	  <Threlte.Mesh
		geometry={new Three.PlaneGeometry(20, 20)}
		material={new Three.MeshStandardMaterial({
		  color: 'white',
		  side: Three.DoubleSide,
		})}
		rotation={{ x: Utils.DEG2RAD * 90 }}
		receiveShadow
	  />
	  <Threlte.Object3DInstance object={gridHelper} />
	  <Threlte.Object3DInstance object={axesHelper} />
	</Threlte.Canvas>
  </div>
  
  <style>
	.scene {
	  width: 100%;
	  height: 100%;
	  position: absolute;
	  inset: 0;
	  background: bg-gradient-to-br variant-gradient-primary-secondary;
	  background-attachment: fixed;
	}
  </style>