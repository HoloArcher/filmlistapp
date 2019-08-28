<template>
    <v-dialog
        v-model="item.on"
         fullscreen 
         overlay="true"
        max-width="500px"
        transition="dialog-transition"
    >
        <h1>
            {{ item }}
        </h1>
    </v-dialog>

</template>



<script>
export default {
    props: ["item"],
    data() {return {

    }}
}
</script>