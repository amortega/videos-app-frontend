<script setup>
import { ref } from 'vue';
import axios from 'axios';
import Alerts from './Alerts.vue';
import VideoDetail from './VideoDetail.vue';

const apiUrl = import.meta.env.VITE_API_URL || 'http://localhost:3000/api/v1';
const videos = ref([]);
const url = ref('');
const alert = ref({
    type: '',
    title: '',
    description: '',
    active: false
});

axios({
    method: 'GET',
    url: `${apiUrl}/videos`
})
    .then(response => {
        videos.value = response.data;
        videos.value = videos.value.map((video) => {
            return {
                ...video,
                modal_active: false,
                play_video: false
            }
        });
    })
    .catch(error => {
        console.error(error);
        alert.value = {
            type: 'error',
            title: 'Ha ocurrido un error, intente de nuevo.',
            description: '',
            active: true
        };
    });

const alertActive = (value) => {
    alert.value.active = value;
}

const resetAlert = () => {
    alert.value = {
        type: '',
        title: '',
        description: '',
        active: false
    };
}

const saveVideo = () => {
    if (url.value && url.value !== '') {
        resetAlert();
        axios({
            method: 'POST',
            url: `${apiUrl}/videos`,
            data: {
                youtube_url: url.value
            }
        })
            .then(response => {
                url.value = '';
                videos.value.push(response.data);
                alert.value = {
                    type: 'success',
                    title: 'Video ingresado correctamente.',
                    description: '',
                    active: true
                };
            })
            .catch(error => {
                console.log('error', error.response.data);
                if (error.response.data.statusCode === 400) {
                    alert.value = {
                        type: 'error',
                        title: error.response.data.message,
                        description: '',
                        active: true
                    };
                }
                else {
                    alert.value = {
                        type: 'error',
                        title: 'Ha ocurrido un error, intente mas tarde.',
                        description: '',
                        active: true
                    };
                }
            });
    }
}



</script>

<template>
    <div class="container">
        <h1>Añadir nuevo video</h1>
        <input v-model="url" type="text" name="add-video" id="add-video" placeholder="Añadir" class="
                w-3/4
                h-[50px]
                border-solid
                border
                border-[#0000004D]
                rounded-[5px]
                rounded-r-[unset]
                pl-[15px]">
        <button type="button" @click="saveVideo()" class="
            w-1/4
            h-[54px]
            text-white
            font-bold
            bg-[#136AE4]
            border-solid
            border
            rounded-[6px]
            rounded-l-[unset]	
        ">
            Añadir
        </button>

        <div class="mt-[50px]">
            <Alerts @alert-active="alertActive(value)" :type="alert.type" :title="alert.title"
                :description="alert.description" :active="alert.active" />
        </div>

        <div class="mt-[100px]">
            <VideoDetail v-for="(video, index) in videos" :key="video.id"
                @close-modal-detail-video="video.modal_active = false" @open-modal-detail-video="video.modal_active = true"
                @stop-video-detail="video.play_video = false" @play-video-detail="video.play_video = true" :id="video.id"
                :title="video.title" :description="video.description" :modal_active="video.modal_active"
                :external_id="video.external_id" :thumbnail_image="video.thumbnail_image"
                :format_duration="video.format_duration" :play_video="video.play_video" />
        </div>
    </div>

    <div>


    </div>
    <!-- <div :id="'modalDetailVideo' + index" style="text-align: -webkit-center;" :class="{ 'hidden': !modalActive, 'block bg-black/50': modalActive }" tabindex="-1" aria-hidden="true" class="fixed top-0 left-0 right-0 z-50 w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[100%] max-h-full">
                    <div class="relative w-full max-w-5xl max-h-full mt-[10%]">
                        <div class="relative bg-white rounded-lg shadow">
                            <div class="flex items-start justify-between p-4 rounded-t">
                                <button @click="closeModal()" type="button" class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ml-auto inline-flex justify-center items-center" :data-modal-hide="'modalDetailVideo' + index">
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
                </div> -->
</template>

<style></style>
