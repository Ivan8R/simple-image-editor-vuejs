<template>
  <transition name="modal-animation">
    <div
      v-show="props.modalActive"
      class="fixed top-0 right-0 left-0 z-50 justify-center items-center w-full bg-white"
    >
      <div class="w-3/4 mx-auto flex flex-col items-center justify-center h-screen">
        <div class="relative border rounded-md w-full flex flex-col bg-gray-100">
          <!-- Modal header -->
          <div
            class="w-full flex flex-col items-center justify-between p-2 md:p-2 dark:border-gray-600"
          >
            <div class="w-full flex flex-col">
              <div class="w-full">
                <button
                  type="button"
                  @click="close"
                  class="float-right text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-fit inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
                  data-modal-hide="default-modal"
                >
                  <svg
                    class="w-3 h-3"
                    aria-hidden="true"
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 14 14"
                  >
                    <path
                      stroke="currentColor"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
                    />
                  </svg>
                  <span class="sr-only">Close modal</span>
                </button>
              </div>
              <div class="mx-auto w-fit">
                <button type="button" @click="operator = 'Adjust'" class="p-2 m-2 text-sm">
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512">
                    <path
                      d="M0 416c0 17.7 14.3 32 32 32l54.7 0c12.3 28.3 40.5 48 73.3 48s61-19.7 73.3-48L480 448c17.7 0 32-14.3 32-32s-14.3-32-32-32l-246.7 0c-12.3-28.3-40.5-48-73.3-48s-61 19.7-73.3 48L32 384c-17.7 0-32 14.3-32 32zm128 0a32 32 0 1 1 64 0 32 32 0 1 1 -64 0zM320 256a32 32 0 1 1 64 0 32 32 0 1 1 -64 0zm32-80c-32.8 0-61 19.7-73.3 48L32 224c-17.7 0-32 14.3-32 32s14.3 32 32 32l246.7 0c12.3 28.3 40.5 48 73.3 48s61-19.7 73.3-48l54.7 0c17.7 0 32-14.3 32-32s-14.3-32-32-32l-54.7 0c-12.3-28.3-40.5-48-73.3-48zM192 128a32 32 0 1 1 0-64 32 32 0 1 1 0 64zm73.3-64C253 35.7 224.8 16 192 16s-61 19.7-73.3 48L32 64C14.3 64 0 78.3 0 96s14.3 32 32 32l86.7 0c12.3 28.3 40.5 48 73.3 48s61-19.7 73.3-48L480 128c17.7 0 32-14.3 32-32s-14.3-32-32-32L265.3 64z"
                    />
                  </svg>
                  Adjust
                </button>
                <button type="button" @click="operator = 'Orientation'" class="p-2 m-2 text-sm">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 512 512"
                    width="40px"
                    height="40px"
                    class="w-fit mx-auto"
                  >
                    <path
                      d="M386.3 160H336c-17.7 0-32 14.3-32 32s14.3 32 32 32H464c17.7 0 32-14.3 32-32V64c0-17.7-14.3-32-32-32s-32 14.3-32 32v51.2L414.4 97.6c-87.5-87.5-229.3-87.5-316.8 0s-87.5 229.3 0 316.8s229.3 87.5 316.8 0c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0c-62.5 62.5-163.8 62.5-226.3 0s-62.5-163.8 0-226.3s163.8-62.5 226.3 0L386.3 160z"
                    />
                  </svg>
                  Orientation
                </button>
                <button
                  type="button"
                  @click=";[(operator = 'Crop'), addCrop()]"
                  class="p-2 m-2 text-sm"
                >
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 512 512"
                    width="40px"
                    height="40px"
                    class="w-fit mx-auto"
                  >
                    <path
                      d="M448 109.3l54.6-54.6c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L402.7 64 160 64v64l178.7 0L128 338.7V32c0-17.7-14.3-32-32-32S64 14.3 64 32V64H32C14.3 64 0 78.3 0 96s14.3 32 32 32H64V384c0 35.3 28.7 64 64 64H352V384H173.3L384 173.3 384 480c0 17.7 14.3 32 32 32s32-14.3 32-32V448h32c17.7 0 32-14.3 32-32s-14.3-32-32-32H448l0-274.7z"
                    />
                  </svg>
                  Crop
                </button>
              </div>
            </div>
            <div class="w-full h-20">
              <div v-if="operator === 'Adjust'" class="flex">
                <div class="flex-1 ml-1 mr-1">
                  <label
                    for="default-range"
                    class="w-fit mx-auto block mb-2 text-sm text-gray-900 dark:text-white"
                    >Brightness</label
                  >
                  <input
                    id="default-range"
                    type="range"
                    min="-255"
                    max="255"
                    class="w-fit block mx-auto h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700"
                    v-model="brightness"
                    v-on:change="brightnessApply"
                  />
                </div>
                <div class="flex-1 ml-1 mr-1">
                  <label
                    for="default-range"
                    class="block w-fit mx-auto mb-2 text-sm text-gray-900 dark:text-white"
                    >Contrast</label
                  >
                  <input
                    id="default-range"
                    type="range"
                    min="-255"
                    max="255"
                    class="w-fit block mx-auto h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700"
                    v-model="contrast"
                    v-on:change="contrastApply"
                  />
                </div>
                <div class="flex-1 ml-1 mr-1">
                  <label
                    for="default-range"
                    class="w-fit mx-auto block mb-2 text-sm font-medium text-gray-900 dark:text-white"
                    >Saturation</label
                  >
                  <input
                    id="default-range"
                    type="range"
                    min="-255"
                    max="255"
                    class="w-fit mx-auto block h-2 bg-gray-200 rounded-lg appearance-none cursor-pointer dark:bg-gray-700"
                    v-model="saturation"
                    v-on:change="saturationApply"
                  />
                </div>
              </div>
              <div v-if="operator === 'Orientation'" class="flex flex-col">
                <div class="w-fitt block mx-auto">
                  <button
                    type="button"
                    class="text-black bg-white hover:bg-gray-200 hover:border border text-sm rounded-md px-3 py-2 text-center inline-flex items-center me-2 mb-2"
                    @click="rotateMinus90"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 512 512"
                      width="15px"
                      height="15px"
                      class="mr-2"
                    >
                      <path
                        d="M48.5 224H40c-13.3 0-24-10.7-24-24V72c0-9.7 5.8-18.5 14.8-22.2s19.3-1.7 26.2 5.2L98.6 96.6c87.6-86.5 228.7-86.2 315.8 1c87.5 87.5 87.5 229.3 0 316.8s-229.3 87.5-316.8 0c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0c62.5 62.5 163.8 62.5 226.3 0s62.5-163.8 0-226.3c-62.2-62.2-162.7-62.5-225.3-1L185 183c6.9 6.9 8.9 17.2 5.2 26.2s-12.5 14.8-22.2 14.8H48.5z"
                      />
                    </svg>
                    Rotate left
                  </button>
                  <button
                    type="button"
                    class="text-black bg-white hover:bg-gray-200 hover:border border text-sm rounded-md px-3 py-2 text-center inline-flex items-center me-2 mb-2"
                    @click="rotatePlus90"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 512 512"
                      width="15px"
                      height="15px"
                      class="mr-2"
                    >
                      <path
                        d="M386.3 160H336c-17.7 0-32 14.3-32 32s14.3 32 32 32H464c17.7 0 32-14.3 32-32V64c0-17.7-14.3-32-32-32s-32 14.3-32 32v51.2L414.4 97.6c-87.5-87.5-229.3-87.5-316.8 0s-87.5 229.3 0 316.8s229.3 87.5 316.8 0c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0c-62.5 62.5-163.8 62.5-226.3 0s-62.5-163.8 0-226.3s163.8-62.5 226.3 0L386.3 160z"
                      />
                    </svg>
                    Rotate right
                  </button>
                  <button
                    type="button"
                    class="text-black bg-white hover:bg-gray-200 hover:border border text-sm rounded-md px-3 py-2 text-center inline-flex items-center me-2 mb-2"
                    @click="flipHorizontally"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 512 512"
                      class="w-4 h-4 me-2"
                    >
                      <path
                        d="M406.6 374.6l96-96c12.5-12.5 12.5-32.8 0-45.3l-96-96c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3L402.7 224l-293.5 0 41.4-41.4c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0l-96 96c-12.5 12.5-12.5 32.8 0 45.3l96 96c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3L109.3 288l293.5 0-41.4 41.4c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0z"
                      />
                    </svg>
                    Flip Horizontally
                  </button>
                  <button
                    type="button"
                    class="text-black bg-white hover:bg-gray-200 hover:border border text-sm rounded-md px-3 py-2 text-center inline-flex items-center me-2 mb-2"
                    @click="flipVertically"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      viewBox="0 0 320 512"
                      class="w-4 h-4 me-2"
                    >
                      <path
                        d="M182.6 9.4c-12.5-12.5-32.8-12.5-45.3 0l-96 96c-12.5 12.5-12.5 32.8 0 45.3s32.8 12.5 45.3 0L128 109.3V402.7L86.6 361.4c-12.5-12.5-32.8-12.5-45.3 0s-12.5 32.8 0 45.3l96 96c12.5 12.5 32.8 12.5 45.3 0l96-96c12.5-12.5 12.5-32.8 0-45.3s-32.8-12.5-45.3 0L192 402.7V109.3l41.4 41.4c12.5 12.5 32.8 12.5 45.3 0s12.5-32.8 0-45.3l-96-96z"
                      />
                    </svg>
                    Flip Vertically
                  </button>
                </div>
                <div class="w-full">
                  <div class="relative items-center block w-fit mx-auto">
                    <button
                      type="button"
                      id="decrement-button"
                      data-input-counter-decrement="counter-input"
                      class="flex-shrink-0 bg-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:border-gray-600 hover:bg-gray-200 inline-flex items-center justify-center border border-gray-300 rounded-md h-5 w-5 focus:ring-gray-100 dark:focus:ring-gray-700 focus:ring-2 focus:outline-none"
                      @click="rotateMinus"
                    >
                      <svg
                        class="w-2.5 h-2.5 text-gray-900 dark:text-white"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 18 2"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M1 1h16"
                        />
                      </svg>
                    </button>
                    <input
                      id="counter-input"
                      readonly="true"
                      data-input-counter
                      class="p-2 text-gray-900 dark:text-white border-0 bg-transparent text-sm font-normal focus:outline-none text-center"
                      placeholder=""
                      v-model="angelRotation"
                      type="number"
                      min="-359"
                      max="359"
                      required
                    />
                    <button
                      type="button"
                      id="increment-button"
                      data-input-counter-increment="counter-input"
                      class="flex-shrink-0 bg-gray-100 dark:bg-gray-700 dark:hover:bg-gray-600 dark:border-gray-600 hover:bg-gray-200 inline-flex items-center justify-center border border-gray-300 rounded-md h-5 w-5 focus:ring-gray-100 dark:focus:ring-gray-700 focus:ring-2 focus:outline-none"
                      @click="rotatePlus"
                    >
                      <svg
                        class="w-2.5 h-2.5 text-gray-900 dark:text-white"
                        aria-hidden="true"
                        xmlns="http://www.w3.org/2000/svg"
                        fill="none"
                        viewBox="0 0 18 18"
                      >
                        <path
                          stroke="currentColor"
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M9 1v16M1 9h16"
                        />
                      </svg>
                    </button>
                  </div>
                </div>
              </div>
              <div v-if="operator === 'Crop'" class="block w-fit mx-auto"></div>
            </div>
          </div>
          <!-- Modal body -->
          <div class="p-2 grow w-full h-fit">
            <canvas ref="canvasReference" />
          </div>
          <!-- Modal footer -->
          <div
            class="flex justify-end items-center p-2 md:p-2 border-gray-200 rounded-b dark:border-gray-600"
          >
            <div v-if="operator">
              <div v-if="operator === 'Adjust'">
                <button
                  class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-yellow-300 font-medium text-sm px-5 py-2.5 mb-2 rounded-l"
                  @click="resetAdjust"
                >
                  Cancel
                </button>

                <button
                  class="focus:outline-none text-white bg-green-500 hover:bg-green-500 focus:ring-green-300 font-medium rounded-r text-sm px-5 py-2.5 me-2 mb-2"
                  @click="operator = ''"
                >
                  Apply
                </button>
              </div>
              <div v-if="operator === 'Orientation'">
                <button
                  class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-yellow-300 font-medium text-sm px-5 py-2.5 mb-2 rounded-l"
                  @click="operator = ''"
                >
                  Cancel
                </button>

                <button
                  class="focus:outline-none text-white bg-green-500 hover:bg-green-500 focus:ring-green-300 font-medium rounded-r text-sm px-5 py-2.5 me-2 mb-2"
                  @click="operator = ''"
                >
                  Apply
                </button>
              </div>
              <div v-if="operator === 'Crop'">
                <button
                  class="focus:outline-none text-white bg-yellow-400 hover:bg-yellow-500 focus:ring-yellow-300 font-medium text-sm px-5 py-2.5 mb-2 rounded-l"
                  @click=";[(operator = ''), resetCrop()]"
                >
                  Cancel
                </button>

                <button
                  class="focus:outline-none text-white bg-green-500 hover:bg-green-500 focus:ring-green-300 font-medium rounded-r text-sm px-5 py-2.5 me-2 mb-2"
                  @click=";[(operator = ''), cropArea()]"
                >
                  Apply
                </button>
              </div>
            </div>
            <div v-else>
              <button
                @click="save"
                type="button"
                class="focus:outline-none text-white bg-green-300 hover:bg-green-500 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2"
              >
                Save
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script setup lang="ts">
  import { fabric } from 'fabric'
  import { ref, onMounted, watch } from 'vue'
  const operator = ref<string>('')
  const brightness = ref(0)
  const contrast = ref(0)
  const saturation = ref(0)
  const angelRotation = ref(0)
  const canvasReference = ref()
  let canvas: any = null
  let filters: any = null
  let cropZone: any = null

  watch(
    () => operator.value,
    (operator) => {
      if (operator == 'Orientation' || operator == 'Adjust') {
        resetCrop()
      }
    }
  )

  onMounted(() => {
    if (props.image) {
      filters = {
        brightness: new fabric.Image.filters.Brightness(),
        saturation: new fabric.Image.filters.Saturation(),
        contrast: new fabric.Image.filters.Contrast()
      }
      canvas = new fabric.Canvas(canvasReference.value, {
        containerClass: 'w-fit h-fit block mx-auto flex',
        isDrawingMode: false
      })
      canvas.setWidth(400)
      canvas.setHeight(400)
      var minX = 0
      var maxX = 400
      var minY = 0
      var maxY = 400

      canvas.on('object:moving', function (e: any) {
        var obj = e.target
        obj.setCoords()
        var b = obj.getBoundingRect()
        if (!(b.left >= minX && maxX >= b.left + b.width)) {
          obj.left = obj.lastLeft
          obj.scaleX = obj.lastScaleX
          obj.scaleY = obj.lastScaleY
        } else {
          obj.lastLeft = obj.left
          obj.lastScaleX = obj.scaleX
        }
        if (!(maxY >= b.top + b.height && b.top >= minY)) {
          obj.top = obj.lastTop
          obj.scaleX = obj.lastScaleX
          obj.scaleY = obj.lastScaleY
        } else {
          obj.lastTop = obj.top
          obj.lastScaleY = obj.scaleY
        }
      })

      fabric.Image.fromURL(props.image, (_img: any) => {
        _img._element.crossOrigin = 'anonymous'
        _img.set({
          top: 0,
          left: 0,
          selectable: false,
          hoverCursor: 'default'
        })
        _img.filters.push(filters.brightness)
        _img.filters.push(filters.saturation)
        _img.filters.push(filters.contrast)

        _img.scaleToWidth(400, false)
        canvas.centerObject(_img)
        canvas.add(_img)
      })
    }
    canvas.renderAll()
  })
  const props = defineProps<{
    modalActive: boolean
    image: string
  }>()
  const emit = defineEmits(['close'])
  const close = () => {
    operator.value = ''
    emit('close')
  }
  const save = () => {
    let w = cropZone.aCoords.tr.x - cropZone.aCoords.tl.x
    let h = cropZone.aCoords.bl.y - cropZone.aCoords.tl.y
    const base64 = canvas.toDataURL({
      format: 'webp',
      enableRetinaScaling: true,
      left: cropZone.left,
      top: cropZone.top,
      height: h,
      width: w
    })
    var a = document.createElement('a')
    a.href = base64
    a.download = 'imagezxczxzxcxzcz.webp'
    a.click()
  }

  function brightnessApply() {
    filters.brightness.brightness = brightness.value * 0.001
    canvas.getObjects()[0].applyFilters()
    canvas.renderAll()
    console.log(canvas.toJSON())
  }
  function contrastApply() {
    filters.contrast.contrast = contrast.value * 0.001
    canvas.getObjects()[0].applyFilters()
    canvas.renderAll()
  }
  function saturationApply() {
    filters.saturation.saturation = saturation.value * 0.001
    canvas.getObjects()[0].applyFilters()
    canvas.renderAll()
  }

  function resetAdjust() {
    operator.value = ''
    filters.brightness.brightness = 0
    filters.contrast.contrast = 0
    filters.saturation.saturation = 0
    brightness.value = 0
    contrast.value = 0
    saturation.value = 0
    canvas.getObjects()[0].applyFilters()
    canvas.renderAll()
  }

  function rotateMinus() {
    angelRotation.value--
    canvas.getObjects()[0]._setOriginToCenter()
    canvas.getObjects()[0].set('angle', angelRotation.value)
    canvas.renderAll()
  }
  function rotatePlus() {
    angelRotation.value++
    canvas.getObjects()[0]._setOriginToCenter()
    canvas.getObjects()[0].set('angle', angelRotation.value)
    canvas.renderAll()
  }

  function rotatePlus90() {
    let obj = canvas.getObjects()[0]
    let angle = obj.get('angle')
    let newAngle = angle + 90
    canvas.getObjects()[0]._setOriginToCenter()
    canvas.getObjects()[0].set('angle', newAngle)
    canvas.renderAll()
    angelRotation.value = angelRotation.value + 90
  }

  function rotateMinus90() {
    let obj = canvas.getObjects()[0]
    let angle = obj.get('angle')
    let newAngle = angle - 90
    canvas.getObjects()[0]._setOriginToCenter()
    canvas.getObjects()[0].set('angle', newAngle)
    canvas.renderAll()
    angelRotation.value = angelRotation.value - 90
  }
  function flipHorizontally() {
    let obj = canvas.getObjects()[0]
    obj.flipX = !obj.flipX
    canvas.renderAll()
  }
  function flipVertically() {
    let obj = canvas.getObjects()[0]
    obj.flipY = !obj.flipY
    canvas.renderAll()
  }

  function addCrop() {
    let obj = canvas.getObjects()[0]
    cropZone = new fabric.Rect({
      width: canvas.getObjects()[0].width / 8,
      height: canvas.getObjects()[0].height / 8,
      left: (obj.aCoords.tr.x - obj.aCoords.tl.x - 200) / 2,
      top: (obj.aCoords.bl.y - obj.aCoords.tl.y) / 2,
      opacity: 0.2,
      hasRotatingPoint: false,
      transparentCorners: false,
      cornerColor: 'white',
      cornerStrokeColor: 'black',
      borderColor: 'black',
      stroke: 'black',
      cornerStyle: 'circle',
      borderDashArray: [5, 5],
      absolutePositioned: true
    })
    canvas.add(cropZone)
    canvas.setActiveObject(canvas.item(1))
  }
  function resetCrop() {
    canvas.remove(cropZone)
    canvas.discardActiveObject().renderAll()
  }

  function cropArea() {
    canvas.getObjects()[0].clipPath = cropZone
    canvas.remove(cropZone)
    canvas.discardActiveObject().renderAll()
  }
</script>

<style scoped>
  .modal-animation-enter-active,
  .modal-animation-leave-active {
    transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
  }

  .modal-animation-enter-from,
  .modal-animation-leave-to {
    opacity: 0;
  }

  .modal-animation-inner-enter-active {
    transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
  }

  .modal-animation-inner-leave-active {
    transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
  }

  .modal-animation-inner-enter-from {
    opacity: 0;
    transform: scale(0.8);
  }

  .modal-animation-inner-leave-to {
    transform: scale(0.8);
  }
</style>
