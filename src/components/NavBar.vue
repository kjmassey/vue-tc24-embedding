<template>
    <div>
        <v-app-bar color="#2A5783">
            <div class="d-flex align-center pl-4">
                <v-icon icon="mdi-library" size="36"></v-icon>
                <div class="text-h6 pl-4">
                    #DataFam Super Real Library
                </div>
            </div>
            <v-spacer></v-spacer>
            <div class="d-flex align-center pr-4">
                <v-btn variant="flat" density="comfortable" class="mr-12" color="success"
                    v-if="this.$props.booksToCheckout.length > 0" @click="$emit('showCheckoutModal')">
                    <v-icon icon="mdi-cart" class="pr-1"></v-icon>Checkout
                    ({{
                        this.$props.booksToCheckout.length }})</v-btn>
                <v-avatar color="white" class="text-h6">KM</v-avatar>
            </div>

        </v-app-bar>
    </div>

</template>

<script>
export default {
    props: { booksToCheckout: { type: Array, default: () => [] } },
    data: () => ({
    })
}
</script>

<style></style>