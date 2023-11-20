<script lang="ts">
  import { T, useThrelte } from '@threlte/core'
  import { OrbitControls, Environment } from '@threlte/extras'
  import Map from './Map.svelte'
  import { Fog, Vector3 } from 'three'
  import { onMount } from 'svelte'

  export let list, target

  const { scene } = useThrelte()
  scene.fog = new Fog(0xffffff, 10, 50)

  onMount(() => {
    list = scene
  })

  let camPos = [3, 2, 3]

  let isRotate = true

  let camera, controls

  $: if(target) {
    console.log(target)

    target.geometry.computeBoundingBox()
    let centroid = new Vector3()

    centroid.addVectors( target.geometry.boundingBox.min, target.geometry.boundingBox.max );
    centroid.multiplyScalar( 0.5 )
    centroid.applyMatrix4( target.matrixWorld )

    camera.position.set(centroid.x+1, 1, centroid.z+1)
    controls.target = centroid
  }
  else {
    if(camera)
    camera.position.set(3, 2, 3)
  }

</script>

<T.PerspectiveCamera
  makeDefault
  fov={50}
  on:create={({ ref }) => {
    camera = ref
  }}
>
  <OrbitControls enableDamping autoRotate={isRotate} on:create={({ ref }) => {
    controls = ref
  }} />
</T.PerspectiveCamera>

<Map position={[-8, 0, 7]} />

<Environment
  path="./"
  files="kloofendal_43d_clear_puresky_1k.hdr"
/>

<T.Mesh position={[0, -0.1, 0]}>
  <T.BoxGeometry args={[200, 0.1, 200]} />
  <T.MeshBasicMaterial color="#DDF8B4" />
</T.Mesh>

<T.HemisphereLight args={[0xffffff, 0x080820, 4]}/>
