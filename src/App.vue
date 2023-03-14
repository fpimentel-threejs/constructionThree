<template>

  <div id="htmlPage">
    <div v-if="!started" id="homePage" class="">
      <transition name ="fade" appear>
        <div id="firsthead" class="text-purple-600 text-2xl">ALL YOUR 3D NEEDS IN</div>
      </transition>
      <transition name ="fade" appear>
        <div id="secondhead" class="text-purple-600 text-3xl">PLACE</div>
      </transition>
      <transition name ="fade" appear>
        <button class="px-12 btn btn-circle glass border-solid border-10" id="startButton"
                @click="
                    //move cam
                    gsap.to(camPos, {
                    x: 0, y:-55, z:35, duration: .8});
                    //move mesh
                    gsap.to(meshPos, {
                    x: 50, y:20, duration: .8});
                    //if started
                    started = true
                    ">START
        </button>
      </transition>
      <transition name ="fade" appear>
        <div id="signature" class="text-purple-600 text-xs">By Fernando Pimentel</div>
      </transition>
    </div>

    <transition name ="fade" appear>
      <div v-if="letterPicked === 'O' && started" id="O1head" class="text-purple-600 text-xl">NOT ONLY CAN YOUR 3D WEBSITE
        BE CREATED, BUT 3D ASSETS CAN BE PROVIDED TO YOU AS WELL</div>
    </transition>
    <transition name ="fade" appear>
      <div v-if="letterPicked === 'O' && started" id="O2head" class="text-purple-600 text-xl">ANIMATIONS AND ARMATURES CAN
        BE IMPLEMENTED DIRECTLY INTO THE ASSETS UPON CREATION, OR BE APPLIED WITHIN YOUR WEBSITE AFTER THE ASSETS HAVE
        BEEN CREATED</div>
    </transition>
    <transition name ="fade" appear>
      <div v-if="letterPicked === 'N'" id="Nhead" class="text-purple-600 text-xl bg-black">I PROVIDE COMPLEX
        MODELS SUCH AS VEGETATION...</div>
    </transition>
    <transition name ="fade" appear>
      <div v-if="letterPicked ==='E'" id="Ehead" class="text-purple-600 text-xl">...AND GEOMETRIC
        MODELS SUCH AS BUILDINGS, VEHICLES, ETC.</div>
    </transition>
    <transition name ="fade" appear>
      <div v-if="letterPicked ==='E'" id="Ehead2" class="text-purple-600 text-xl">CONTACT ME</div>
    </transition>

    <transition name ="fade" appear>
      <div v-if="letterPicked ==='E'" id="contactsvg" class="grid grid-cols-3">
        <div id="links"><a href="https://github.com/fpimentel-threejs"><img src="/assets/githublogo.svg"/></a></div>
        <div id="links"><a href="https://www.linkedin.com/in/fpimentel/"><img src="/assets/linkedinlogo.svg"/></a></div>
        <div id="links"><a href="https://www.instagram.com/rxnando/"><img src="/assets/instagramlogo.svg"/></a></div>
      </div>
    </transition>
    <transition name ="fade" appear>
      <div id="radio" class="grid grid-cols-3">
        <input v-if="started" id="radioBut" v-model="letterPicked" value="O" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 0, duration: .3})" checked />
        <input v-if="started" id="radioBut" v-model="letterPicked" value="N" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 35, duration: .3})"/>
        <input v-if="started" id="radioBut" v-model="letterPicked" value="E" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 67, duration: .3})"/>
      </div>
    </transition>
  </div>

  <Renderer ref="rendererC" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera :position="{ x: camPos.x, y: camPos.y, z: camPos.z }" />
    <Scene ref="sceneRef" :background="0xffffff">
      <PointLight :intensity=".4" :position="{ x:-50, z: 50 }" />
      <AmbientLight :intensity=".8" />

      <Group
          :position="{x: -gsapers.moveScene}"
      >
        <div v-if="camPos.z === 35">
          <GltfModel
              src="/assets/bigtree.gltf"
              @load="onReady"
              @progress="onProgress"
              @error="onError"
              :position="{ x: 46,y: 7,z: 4.3}"
              :rotation="{x: 90*Math.PI/180}"
          />
          <GltfModel
              src="/assets/weirdtree.gltf"
              @load="onReady"
              @progress="onProgress"
              @error="onError"
              :position="{ x: 26,y: -5,z: 4.3}"
              :rotation="{x: 90*Math.PI/180}"
          />
          <Text
              text="N"
              font-src="/assets/Ronda_Regular.json"
              align="center"
              :size="30"
              :height=".1"
              :position="{ x: 35.5, y: 0,z: 4 }"

          >
              <LambertMaterial :color="0x98c931"/>
          </Text>
          <GltfModel
              src="/assets/Ebuildings.gltf"
              @load="onReady"
              @progress="onProgress"
              @error="onError"
              :position="{ x: 69,y: -.5,z: 3.8}"
              :rotation="{x: 270*Math.PI/180, y: -90*Math.PI/180, z: Math.PI}"
          />
          <Box
              :height="29"
              :width="3"
              :depth=".3"
              :position="{ x: 61.5,y: 0,z: 4.2}"
          >
              <LambertMaterial :color="0xb6b6b6"/>
          </Box>
          <Box
              :height="29"
              :width="4"
              :depth=".1"
              :position="{ x: 58,y: 0,z: 4}"
          >
              <LambertMaterial :color="0x212121"/>
          </Box>
          <Box
              :height="29"
              :width=".2"
              :depth=".1"
              :position="{ x: 59,y: 0,z: 4.01}"
          >
              <LambertMaterial :color="0xffffff"/>
          </Box>
          <Torus
              :radius="12.7"
              :tube="3.8"
              :radialSegments="2"
              :tubularSegments="100"
              :position="{z: 4.05}"
          >
            <LambertMaterial :color="0x292620"/>
          </Torus>
          <Torus
            :radius="16"
            :tube=".1"
            :radialSegments="2"
            :tubularSegments="100"
            :position="{z: 4.06}"
          >
            <LambertMaterial :color="0xffffff"/>
          </Torus>
          <Torus
            :radius="9.3"
            :tube=".1"
            :radialSegments="2"
            :tubularSegments="100"
            :position="{z: 4.06}"
          >
            <LambertMaterial :color="0xffffff"/>
          </Torus>
          <Torus
            :radius="13"
            :tube=".1"
            :radialSegments="2"
            :tubularSegments="100"
            :position="{z: 4.06}"
          >
            <LambertMaterial :color="0xffe200"/>
          </Torus>
          <Torus
            :radius="12.5"
            :tube=".1"
            :radialSegments="2"
            :tubularSegments="100"
            :position="{z: 4.06}"
          >
            <LambertMaterial :color="0xffe200"/>
          </Torus>
        </div>

        <Group
          ref="carRotation2"
        >
          <GltfModel v-if="started"
                   ref="carRef"
                   src="/assets/chatgptcar2.gltf"
                   @load="onReady"
                   @progress="onProgress"
                   @error="onError"
                   :position="{ x: -14.5,y: 0,z: 4.9}"
          />
        </Group>
        <Group
          ref="carRotation"
        >
        <GltfModel v-if="started"
          ref="carRef"
          src="/assets/chatgptcar.gltf"
          @load="onReady"
          @progress="onProgress"
          @error="onError"
          :position="{ x: -10.5,y: 0,z: 4.9}"
        />
        </Group>
        <Text
          text="O"
          font-src="/assets/Ronda_Regular.json"
          align="center"
          :size="35"
          :height="8"
          :position="{ x: -50 + meshPos.x, y: 20 - meshPos.y}"
        >
          <LambertMaterial :color="0xd6de3b"/>
        </Text>

        <Text
          text="NE"
          font-src="/assets/Ronda_Regular.json"
          align="center"
          :size="30"
          :height="8"
          :position="{ x: 0 + meshPos.x, y: 20 - meshPos.y }"
        >
          <LambertMaterial :color="0xd6de3b"/>
        </Text>
      </Group>



    </Scene>
  </Renderer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { GridHelper } from 'three'
import { AmbientLight, Box, Torus, Group, GltfModel, Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs'
import gsap from 'gsap'

const meshPos = ref({
  x: 0,
  y: 0
})

const camPos = ref({
  x: 0,
  y: 0,
  z: 200
})

const gsapers = ref({
  moveScene: 0
})

const rendererC = ref()
const wheel = ref()
const sceneRef = ref()
const carRef = ref()
const carRotation = ref()
const carRotation2 = ref()
const letterPicked = ref('O')
const started = ref(false)
const gridHelper = new GridHelper( 1000, 400, 0xe5e4e2, 0xe5e4e2 );

onMounted(() => {
  const renderer = rendererC.value
  const sceneR = sceneRef.value
  const carRot = carRotation.value
  const carRot2 = carRotation2.value

  //sceneR.add(gridHelper)
  gridHelper.rotation.x = 90 * Math.PI / 180
  gridHelper.position.z = -20

  renderer.onBeforeRender(() => {
    carRot.group.rotation.z -= .02
    carRot2.group.rotation.z += .015
  })

})
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@600&display=swap');
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@200&display=swap');

#htmlPage{
  position: absolute;
  top: 0;
  left: 0;
  z-index: 50;
  width: 100vw;
  height: 100vh;
}

#homePage{
  position: absolute;
}

#startButton {
  width: 20vw;
  margin: 0 40vw 5vh 35vw;
}

#radio{
  position: absolute;
  width: 100vw;
  margin: 90vh 0;
  height: 5vh;
}

#radioBut{
  margin: 0 15vw;
}

#firsthead {
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6);
  border: solid black 1px;
  margin: 15vh 5vw;
  width: 90vw;
}

#secondhead {
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6);
  border: solid black 1px;
  margin: 40vh 5vw 10vh 5vw;
  width: 90vw;
  padding-left: 45vw;
}

#signature {
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6);
  margin: 0 0 0 80vw;
}

#O1head {
  position: absolute;
  width: 60vw;
  height: 20vh;
  margin: 5vh 5vw;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6)
}

#O2head {
  position: absolute;
  width: 25vw;
  min-width: 30vw;
  height: 60vh;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6);
  margin: 30vh 0 0 5vw;
}

#Nhead {
  position: absolute;
  margin: 5vh 5vw 65vh 5vw;
  width: 30vw;
  height: 20vh;
  z-index: 3;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6);
}

#Ehead {
  position: absolute;
  margin: 5vh 5vw 0 70vw;
  width: 30vw;
  height: 20vh;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6)
}

#Ehead2 {
  position: absolute;
  width: 30vw;
  margin: 30vh 5vw 0 70vw;
  font-family: 'Montserrat', sans-serif;
  border-bottom: solid 1px;
  background-color: rgba(255,255,255,.6)
}

#contactsvg {
  position: absolute;
  width: 30vw;
  margin: 40vh 5vw 0 70vw;
}

#links {
  margin: 1.5vw;
  padding: 1vw;
  background-color: rgba(255,255,255,.6);
  border-radius: 50%;
}

body {
  margin: 0;
}
canvas {
  display: block;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>