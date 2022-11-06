<template>
    <v-dialog
        v-model="dialog"
    >   
        <v-card>
            Popup dialog window
        </v-card>
        <v-btn @click="close">닫기</v-btn>
    </v-dialog>
</template>

<script>
    export default {
        name: 'ErrorPopupDialog',
        data() {
            return {
                dialog: true
            }
        },
        methods: {
            close: function() {
                console.log("close popup dialog");
                this.$emit("closeErrorDialog");
            }
        }
    }
</script>

<style>

</style>