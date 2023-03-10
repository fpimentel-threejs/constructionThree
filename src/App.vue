<template>
  <Renderer ref="rendererC" antialias :orbit-ctrl="{ enabled: false }" resize="window">
    <Camera :position="{ x: camPos.x, y: camPos.y, z: camPos.z }" />
    <Scene ref="sceneRef" :background="0xffffff">
      <PointLight :intensity=".4" :position="{ x:-50, z: 50 }" />
      <AmbientLight :intensity=".8" />

      <Group
          :position="{x: -gsapers.moveScene}"
      >
        <div v-if="camPos.z == 35">
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
      <transition name ="fade" appear>
      <button v-if="!started" class="px-12 btn btn-circle glass border-solid border-10" id="startButton"
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
      <input v-if="started" id="radio1" v-model="letterPicked" value="'O'" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 0, duration: .3})" checked />
      <input v-if="started" id="radio2" v-model="letterPicked" value="'N'" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 35, duration: .3})"/>
      <input v-if="started" id="radio3" v-model="letterPicked" value="'E'" name="letterPicker" type="radio" class="radio radio-success" @click="gsap.to(gsapers, {moveScene: 67, duration: .3})"/>

      <transition name ="fade" appear>
      <div v-if="!started" id="firsthead" class="text-purple-600 text-2xl">ALL YOUR 3D NEEDS IN</div>
      </transition>
      <transition name ="fade" appear>
      <div v-if="!started" id="secondhead" class="text-purple-600 text-3xl">PLACE</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="!started" id="signature" class="text-purple-600 text-xs">By Fernando Pimentel</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 0 && started" id="O1head" class="text-purple-600 text-xl">NOT ONLY CAN YOUR 3D WEBSITE
          BE CREATED, BUT 3D ASSETS CAN BE PROVIDED TO YOU AS WELL</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 0 && started" id="O2head" class="text-purple-600 text-xl">ANIMATIONS AND ARMATURES CAN
          BE IMPLEMENTED DIRECTLY INTO THE ASSETS UPON CREATION, OR BE APPLIED WITHIN YOUR WEBSITE AFTER THE ASSETS HAVE
          BEEN CREATED</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 35 && started" id="Nhead" class="text-purple-600 text-xl bg-black">I PROVIDE COMPLEX
        MODELS SUCH AS VEGETATION...</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 67 && started" id="Ehead" class="text-purple-600 text-xl">...AND GEOMETRIC
        MODELS SUCH AS BUILDINGS, VEHICLES, ETC.</div>
      </transition>
      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 67 && started" id="Ehead2" class="text-purple-600 text-xl">CONTACT ME</div>
      </transition>

      <transition name ="fade" appear>
        <div v-if="gsapers.moveScene == 67 && started" id="contactsvg" class="grid grid-cols-3">
          <div id="links"><a href="https://github.com/fpimentel-threejs"><img src="/assets/githublogo copy.svg"/></a></div>
          <div id="links"><a href="https://www.linkedin.com/in/fpimentel/"><img src="/assets/linkedinlogo copy.svg"/></a></div>
          <div id="links"><a href="https://www.instagram.com/rxnando/"><img src="/assets/instagramlogo copy.svg"/></a></div>
        </div>
      </transition>



    </Scene>
  </Renderer>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { GridHelper } from 'three'
import { Box, Torus, Group, GltfModel, Camera, LambertMaterial, PointLight, Renderer, Scene } from 'troisjs'
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
const letterPicked = ref('')
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

#startButton {
  position: absolute;
  top: 75%;
  left: 45%;
  width: 10%;
  z-index: 3;
}

#radio1 {
  position: absolute;
  left: 30%;
  top: 90%;
  z-index: 3;
}

#radio2 {
  position: absolute;
  left: 45%;
  top: 90%;
  z-index: 3;
}

#radio3 {
  position: absolute;
  left: 60%;
  top: 90%;
  z-index: 3;
}

#firsthead {
  position: absolute;
  left: 10%;
  top: 10%;
  z-index: 3;
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6)
}

#secondhead {
  position: absolute;
  left: 50%;
  top: 60%;
  z-index: 3;
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6)
}

#signature {
  position: absolute;
  left: 85%;
  top: 90%;
  z-index: 3;
  font-family: 'Montserrat', sans-serif;
  background-color: rgba(255,255,255,.6)
}

#O1head {
  position: absolute;
  left: 10%;
  top: 5%;
  width: 60%;
  z-index: 3;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6)
}

#O2head {
  position: absolute;
  left: 10%;
  top: 25%;
  width: 25%;
  z-index: 3;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6)
}

#Nhead {
  position: absolute;
  left: 10%;
  top: 10%;
  width: 20%;
  z-index: 3;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6);
}

#Ehead {
  position: absolute;
  left: 70%;
  top: 10%;
  width: 25%;
  z-index: 3;
  font-family: 'IBM Plex Mono', monospace;
  background-color: rgba(255,255,255,.6)
}

#Ehead2 {
  position: absolute;
  left: 75%;
  top: 40%;
  width: 25%;
  z-index: 3;
  font-family: 'Montserrat', sans-serif;
  border-bottom: solid 1px;
  background-color: rgba(255,255,255,.6)
}

#contactsvg {
  position: absolute;
  left: 70%;
  top: 52%;
  width: 25%;
  z-index: 3;
}

#links {
  width: 65%;
  padding: 10%;
  background-color: rgba(255,255,255,.4);
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