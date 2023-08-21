<template>
  <Teleport to="body">
    <Transition name="modal-outer">
      <div v-show="modalActive"
        class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8">
        <Transition name="modal-inner">
          <div v-if="modalActive" class="p-4 bg-white self-start mt-32 max-w-screen-md">
            <button @click="$emit('closeModal'); $emit('stopVideo');" type="button"
              class="float-right text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center">
              <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
              </svg>
              <span class="sr-only">Close modal</span>
            </button>
            <div class="text-black">
              <div class="grid gap-2 grid-cols-2">
                <div>
                  <iframe v-if="props.play_video" :src="'https://www.youtube.com/embed/' + props.external_id + '?autoplay=1'"
                    width="100%" height="250" class="embed-responsive-item" frameborder="0" allow="autoplay"></iframe>
                  <div class="max-w-[492px] max-h-[308px] m-[25px] float-left" v-if="!props.play_video">
                    <div class="relative">
                      <button class="absolute inset-y-0 left-[41%]" @click="$emit('playVideo')">
                        <svg height="54px" width="57px" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg"
                          xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512 512" xml:space="preserve">
                          <polygon style="fill:#FFFFFF;" points="187.368,146.928 187.368,355.8 382.992,251.368 " />
                          <path style="fill:#FF0000;"
                            d="M256,0.376C114.616,0.376,0,114.824,0,256s114.616,255.624,256,255.624S512,397.176,512,256
                                      S397.384,0.376,256,0.376z M184.496,146.928l195.624,104.44L184.496,355.8V146.928z" />
                        </svg>
                      </button>

                      <img :src="props?.thumbnail_image" width="492" height="308">
                      <div
                        :class="{ 'w-[65px]': props.format_duration.length < 12, 'w-[100px]': props.format_duration.length >= 12 }"
                        class="absolute h-[22px] m-[6px] text-[#ffffff] font-bold bg-black opacity-80 rounded-[3px] text-center right-px bottom-px">
                        {{ props?.format_duration }}</div>
                    </div>
                  </div>
                </div>
                <div>
                  <div class="mt-[10px] font-bold">
                    <h1>{{ props.title }}</h1>
                  </div>
                  <div class="mt-[40px] overflow-x-auto max-h-[48vh] text-justify">
                    <h3 class="mr-[10px]">{{ props.description }}</h3>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </Transition>
      </div>
    </Transition>
  </Teleport>
</template>
  
<script setup>
import { ref } from 'vue';
let playVideo = ref(false);
defineEmits(['closeModal', 'stopVideo', 'playVideo']);
const props = defineProps({
  modalActive: {
    type: Boolean,
    default: false,
  },
  title: {
    type: String,
    default: ''
  },
  description: {
    type: String,
    default: ''
  },
  external_id: {
    type: String,
    default: ''
  },
  thumbnail_image: {
    type: String,
    default: ''
  },
  format_duration: {
    type: String,
    default: ''
  },
  play_video: {
    type: Boolean,
    default: false
  },
});

</script>
  
<style scoped>
.modal-outer-enter-active,
.modal-outer-leave-active {
  transition: opacity 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-outer-enter-from,
.modal-outer-leave-to {
  opacity: 0;
}

.modal-inner-enter-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02) 0.15s;
}

.modal-inner-leave-active {
  transition: all 0.3s cubic-bezier(0.52, 0.02, 0.19, 1.02);
}

.modal-inner-enter-from {
  opacity: 0;
  transform: scale(0.8);
}

.modal-inner-leave-to {
  transform: scale(0.8);
}
</style>