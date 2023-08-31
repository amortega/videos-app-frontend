<script setup>
import { ref } from 'vue';
import axios from 'axios';
import Alerts from './Alerts.vue';
import Loader from './icons/Loader.vue';
import VideoDetail from './VideoDetail.vue';
import DeleteModal from './DeleteModal.vue';

const apiUrl = import.meta.env.VITE_API_URL || 'http://localhost:3000/api/v1';
const videos = ref([]);
const url = ref('');
const disabledButtons = ref(true);
const loading = ref(true);
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
                modal_delete_active: false,
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
    })
    .finally(() => {
        disabledButtons.value = false;
        loading.value = false;
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
        disabledButtons.value = true;
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
                url.value = '';
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
            })
            .finally(disabledButtons.value = false);
    }
}

const deleteVideo = (confirm, video) => {
    if (!confirm) {
        video.modal_delete_active = false;
        changeScroll('closeModal');
        return;
    }

    disabledButtons.value = true;
    resetAlert();
    axios({
        method: 'DELETE',
        url: `${apiUrl}/videos/${video.id}`,
    })
        .then(response => {
            const idx = videos.value.findIndex(val => val.id === video.id);
            videos.value.splice(idx, 1);
            changeScroll('closeModal');
            alert.value = {
                type: 'success',
                title: 'Video eliminado correctamente.',
                description: '',
                active: true
            };
        })
        .catch(error => {
            changeScroll('closeModal');
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
        })
        .finally(disabledButtons.value = false);
}

const changeScroll = (action) => {
    if (action === 'openModal') {
        document.body.classList.add("modal-open");
        window.scroll(0, 0);
    }
    else {
        document.body.classList.remove("modal-open");
    }
}

</script>

<template>
    <div class="container mt-6">
        <p class="font-bold	text-2xl mb-6">Añadir nuevo video</p>
        <input :disabled="disabledButtons" v-model="url" type="text" name="add-video" id="add-video" placeholder="Añadir" class="
                w-3/4
                h-[50px]
                border-solid
                border
                border-[#0000004D]
                rounded-[5px]
                rounded-r-[unset]
                pl-[15px]">
        <button type="button" :disabled="disabledButtons" @click="saveVideo()" class="
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
        <div class="flex justify-center">
            <Loader v-if="loading"/>
            <p v-if="!loading && !videos.length">No tienes videos registrados, ¡agrega alguno!</p>
        </div>

        <div v-if="!loading" class="mt-[100px] grid sm:grid-cols-2 sm:gap-2 md:grid-cols-3 md:gap-3 lg:grid-cols-4 :gap-4">
            <template v-for="(video, index) in videos" :key="video.id" :index="index">
                <VideoDetail 
                @close-modal-detail-video="video.modal_active = false; changeScroll('closeModal');" 
                @open-modal-detail-video="video.modal_active = true; changeScroll('openModal');"
                @stop-video-detail="video.play_video = false" 
                @play-video-detail="video.play_video = true"
                @open-delete-modal="video.modal_delete_active = true; changeScroll('openModal');" 
                :id="video.id"
                :title="video.title" 
                :description="video.description" 
                :modal_active="video.modal_active"
                :external_id="video.external_id" 
                :thumbnail_image="video.thumbnail_image"
                :format_duration="video.format_duration" 
                :play_video="video.play_video" 
                :embed_url="video.embed_url"
                />
                
                <DeleteModal 
                @close-modal-delete="video.modal_delete_active = false; changeScroll('closeModal');"
                @delete-video="(val) => deleteVideo(val, video)"
                :modal_active="video.modal_delete_active"
                />
            </template>
        </div>
    </div>

    <div>


    </div>
</template>

<style></style>
