<template>
    <div class="flex justify-center items-center p-4" v-bind="$attrs">
        <div class="flex items-center mx-auto">
            <input v-model="name" type="text" placeholder="Enter name"
                class="p-2.5 w-1/2 text-sm text-gray-700 bg-white rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500">
            <button @click="sendData" class="ml-2 bg-transparent hover:bg-gray-500 text-gray-700 font-semibold hover:text-white py-2 px-4 border border-gray-500 hover:border-transparent rounded">Register</button>
        </div>
    </div>
    <p v-if="showWarning" class="text-red-500">名前を入力してください。</p>
</template>
  
<script>
import axios from 'axios';
export default {
    emits: ['registration-success'],
    data() {
        return {
            name: '',
            cardId: '',
            responseMessage: '',
            registrationSuccessful: false,
            showWarning: false, // 警告を表示するためのフラグ
        };
    },
    methods: {
        sendData() {
            if (this.name.trim() === '') {
                // テキストボックスが空の場合、警告を表示して送信しない
                this.showWarning = true;
                return;
            }

            axios.post('/api/user', { name: this.name })
                .then(response => {
                    this.cardId = response.data.cardId;
                    this.registrationSuccessful = true;
                    this.$emit('registration-success', { cardId: this.cardId, name: this.name });
                    this.name = ''; // 成功後にテキストボックスをクリア
                    this.showWarning = false;
                })
                .catch(error => {
                    this.responseMessage = error.response.data.error;
                });
        },
    }
};

</script>
