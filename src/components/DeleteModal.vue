<template>
    <Teleport to="body">
      <Transition name="modal-outer">
        <div v-show="modal_active"
          class="absolute w-full bg-black bg-opacity-30 h-screen top-0 left-0 flex justify-center px-8">
          <Transition name="modal-inner">
            <div v-if="modal_active" class="p-4 bg-white self-start mt-32 w-2/4 h-[25vh] rounded">
              <button @click="$emit('closeModalDelete');" type="button"
                class="float-right text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center">
                <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                  <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                </svg>
                <span class="sr-only">Close modal</span>
              </button>
              <div class="text-black">
                <p class="mt-[5%] ml-[5%] font-bold">¿Seguro que quieres eliminar este video?</p>
                <div class="ml-[10%] mt-[5%] mr-[1%] text-right">
                    <button @click="$emit('deleteVideo', false)" type="button" class="mr-[3%] w-2/5 h-[45px] text-[#136AE4] font-bold bg-white border-solid border rounded-[8px]">
                        Cancelar
                    </button>
                    <button @click="$emit('deleteVideo', true)" type="button" class="w-2/5 h-[45px] text-white font-bold bg-[#136AE4] border-solid border rounded-[8px]">
                        Eliminar
                    </button>
                </div>
                
              </div>
            </div>
          </Transition>
        </div>
      </Transition>
    </Teleport>
  </template>
    
  <script setup>
  defineEmits(['deleteVideo', 'closeModalDelete']);
  const props = defineProps({
    modal_active: {
        type: Boolean,
        default: false,
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