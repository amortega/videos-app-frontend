<script setup>
    import { ref } from 'vue';
    const modalActive = ref(false);
    const items = ref([
        { message: 'Foo' }, 
        { message: 'Bar' },
        { message: 'Foo' }, 
        { message: 'Bar' },
        { message: 'Foo' }, 
        { message: 'Bar' },
        { message: 'Foo' }, 
        { message: 'Bar' },
        { message: 'Foo' }, 
        { message: 'Bar' },
    ]);
    const openModal = () => {
        modalActive.value = true;
    }
    const closeModal = () => {
        modalActive.value = false;
    }
</script>

<template>
    

    <div class="container">
        <h1>Añadir nuevo video</h1>
        <input 
            type="text" 
            name="add-video" 
            id="add-video" 
            placeholder="Añadir"
            class="
                w-3/4
                h-[50px]
                border-solid
                border
                border-[#0000004D]
                rounded-[5px]
                rounded-r-[unset]
                pl-[15px]"
        >
        <button type="button"
        class="
            w-1/4
            h-[54px]
            text-white
            font-bold
            bg-[#136AE4]
            border-solid
            border
            rounded-[6px]
            rounded-l-[unset]	
        "
        >
            Añadir
        </button>
        <div class="mt-[100px]">
            <div class="max-w-[263px] max-h-[150px] m-[25px] float-left" v-for="item in items">
                <div class="relative">
                    <div class="absolute w-[24px] h-[24px] m-[7px] text-[#ffffff] font-medium bg-black opacity-80 rounded-[3px] text-center right-px cursor-pointer">x</div>
                    <img src="../assets/mario.jpeg" width="263" height="150">
                    <div class="absolute w-[60px] h-[22px] m-[6px] text-[#ffffff] font-bold bg-black opacity-80 rounded-[3px] text-center right-px bottom-px">0:40</div>
                </div>
            </div>
        </div>


        
    </div>

    <div>
        <!-- Modal toggle -->
        <button @click="modalActive = !modalActive" data-modal-target="defaultModal" data-modal-toggle="defaultModal" class="block text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center" type="button">
            Toggle modal
        </button>

        <div id="defaultModal" style="text-align: -webkit-center;" :class="{ 'hidden': !modalActive, 'block bg-black/50': modalActive }" tabindex="-1" aria-hidden="true" class="fixed top-0 left-0 right-0 z-50 w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[100%] max-h-full">
            <div class="relative w-full max-w-5xl max-h-full mt-[10%]">
                <div class="relative bg-white rounded-lg shadow">
                    <div class="flex items-start justify-between p-4 rounded-t">
                        <button @click="closeModal()" type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center" data-modal-hide="defaultModal">
                            <svg class="w-3 h-3" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14">
                                <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"/>
                            </svg>
                            <span class="sr-only">Close modal</span>
                        </button>
                    </div>
                    <div class="p-6 space-y-6">
                        <div class="grid gap-2 grid-cols-2">
                            <div>
                                <iframe 
                                    src="https://www.youtube.com/embed/1Ud25W4dcUs"
                                    width="100%"
                                    height="250"
                                    class="embed-responsive-item"
                                    frameborder="0"
                                ></iframe>
                            </div>
                            <div>
                                <div class="mt-[10px] font-bold">
                                    <h1>Cómo Ganar Dinero Por Internet Siendo Principiante (Paso a Paso)</h1>
                                </div>
                                <div class="mt-[40px]">
                                    <h3>Descubre el negocio online que puedes empezar sin inversión: https://youtu.be/f8Cjgg3MAw8</h3>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
</template>

<style>

</style>
