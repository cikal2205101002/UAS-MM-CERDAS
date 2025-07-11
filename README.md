<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Large Minecraft House with Moving Steve</title>
    <script src="https://aframe.io/releases/1.5.0/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-environment-component@1.5.x/dist/aframe-environment-component.min.js"></script>
  </head>
  <body>
    <a-scene
      shadow="type: pcfsoft"
      background="color: #a0e9ff"
      fog="type: linear; color: #555; near: 1; far: 50"
    >
      <a-entity position="0 1.6 4">
        <a-camera
          wasd-controls-enabled="true"
          look-controls="pointerLockEnabled: true"
        ></a-camera>
      </a-entity>

      <a-entity
        environment="preset: forest; ground: hills; dressing: trees;"
      ></a-entity>
      <a-sky color="#a0e9ff"></a-sky>

      <a-plane
        rotation="-90 0 0"
        width="300"
        height="300"
        src="https://cdn.aframe.io/a-painter/images/floor.jpg"
        repeat="150 150"
        shadow="receive: true"
      ></a-plane>

      <!-- Minecraft House Model -->
      <a-entity position="0 0 -15">
        <a-gltf-model
          src="winemaker_house_by_hoolss__mekel.glb"
          position="0 0 0"
          rotation="0 45 0"
          scale="0.10 0.10 0.10"
          shadow="cast: true; receive: true"
        ></a-gltf-model>

        <a-gltf-model
          src="bmw_700_minecraft.glb"
          position="1 0.1 5"
          rotation="0 -45 0"
          scale="0.15 0.15 0.15"
          shadow="cast: true"
        ></a-gltf-model>

        <!-- Tempat Duduk -->
        <a-gltf-model
          src="bench_minecraft.glb"
          position="2.5 0.2 -5.2"
          rotation="0 90 0"
          scale="0.25 0.25 0.25"
        ></a-gltf-model>

        <!-- Steve yang bergerak -->
        <a-gltf-model
          id="steve-npc"
          src="steve_from_a_minecraft_movie.glb"
          position="5.5 0.2 -4.5"
          rotation="0 90 0"
          scale="0.40 0.40 0.40"
          steve-fsm
        ></a-gltf-model>

        <!-- Temple -->
        <a-gltf-model
          src="thunderclap_temple.glb"
          position="50 0 80"
          rotation="0 -90 0"
          scale="1.4 1.4 1.4"
          shadow="cast: true; receive: true"
        ></a-gltf-model>

        <!-- Fence -->
        <a-box
          position="-8 1.5 -8"
          width="0.3"
          height="2"
          depth="16"
          color="#8B4513"
        ></a-box>
        <a-box
          position="8 1.5 -8"
          width="0.3"
          height="2"
          depth="16"
          color="#8B4513"
        ></a-box>
        <a-box
          position="0 1.5 -16"
          width="16"
          height="2"
          depth="0.3"
          color="#8B4513"
        ></a-box>

        <!-- Pathway -->
        <a-box
          position="0 0.2 -4"
          width="3"
          height="0.3"
          depth="12"
          color="#A9A9A9"
          rotation="0 90 0"
        ></a-box>

        <!-- Trees -->
        <a-cylinder
          position="-12 2 -12"
          radius="0.5"
          height="4"
          color="#8B4513"
        ></a-cylinder>
        <a-cone
          position="-12 6 -12"
          radius-bottom="2.5"
          height="5"
          color="#228B22"
        ></a-cone>
        <a-cylinder
          position="12 2 -12"
          radius="0.5"
          height="4"
          color="#8B4513"
        ></a-cylinder>
        <a-cone
          position="12 6 -12"
          radius-bottom="2.5"
          height="5"
          color="#228B22"
        ></a-cone>

        <!-- Lights -->
        <a-cylinder
          position="-5 0 -15"
          radius="0.3"
          height="3"
          color="#696969"
        ></a-cylinder>
        <a-sphere position="-5 3 -15" radius="0.5" color="#FFFF00"></a-sphere>
        <a-cylinder
          position="5 0 -15"
          radius="0.3"
          height="3"
          color="#696969"
        ></a-cylinder>
        <a-sphere position="5 3 -15" radius="0.5" color="#FFFF00"></a-sphere>

        <!-- Balkon Dikecilin + Dijauhin -->
        <a-box
          position="0 2.5 -6"
          width="4"
          height="0.2"
          depth="2"
          color="#A0522D"
        ></a-box>
        <a-box
          position="0 1 -6"
          width="0.2"
          height="2"
          depth="0.2"
          color="#8B4513"
        ></a-box>
        <a-box
          position="2 1 -6"
          width="0.2"
          height="2"
          depth="0.2"
          color="#8B4513"
        ></a-box>
        <a-box
          position="-2 1 -6"
          width="0.2"
          height="2"
          depth="0.2"
          color="#8B4513"
        ></a-box>
      </a-entity>

      <!-- Lighting -->
      <a-light type="ambient" color="#ffffff" intensity="0.6"></a-light>
      <a-light
        type="directional"
        position="0 20 10"
        intensity="2.0"
        castShadow="true"
        shadow-camera-top="20"
        shadow-camera-bottom="-20"
        shadow-camera-left="-20"
        shadow-camera-right="20"
      ></a-light>
      <a-light
        type="point"
        position="0 8 -10"
        intensity="1.0"
        color="#FFFF00"
      ></a-light>
      <a-light
        type="point"
        position="-5 5 5"
        intensity="0.7"
        color="#FFA500"
      ></a-light>
    </a-scene>

    <!-- AI FSM untuk Steve -->
    <script>
      AFRAME.registerComponent("steve-fsm", {
        schema: {},
        init: function () {
          this.state = "WALKING_FORWARD";
          this.speed = 0.002;
          this.targetX = 8;
        },
        tick: function (time, timeDelta) {
          let pos = this.el.object3D.position;
          if (this.state === "WALKING_FORWARD") {
            if (pos.x < this.targetX) {
              pos.x += this.speed * timeDelta;
              this.el.object3D.rotation.y = -Math.PI / 2; // hadap kanan
            } else {
              this.state = "WALKING_BACKWARD";
            }
          } else if (this.state === "WALKING_BACKWARD") {
            if (pos.x > 3) {
              pos.x -= this.speed * timeDelta;
              this.el.object3D.rotation.y = Math.PI / 2; // hadap kiri
            } else {
              this.state = "WALKING_FORWARD";
            }
          }
        },
      });
    </script>
  </body>
</html>
