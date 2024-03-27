<template>
    <v-dialog v-model="this.$props.showCheckoutModal" max-width="725px" style="overflow: hidden;" persistent>
        <v-card>
            <v-card-item>
                <v-card-title>
                    <div class="d-flex justify-space-between">
                        <div>{{ checkedOut ? 'Books Reserved' : 'Ready to Checkout?' }}</div>
                        <div>
                            <v-icon icon="mdi-close" color="#2A5783" style="cursor: pointer;"
                                @click="$emit('closeCheckoutModal')"></v-icon>
                        </div>
                    </div>
                </v-card-title>
                <v-card-subtitle>Pick up your selection(s) at the front desk</v-card-subtitle>
            </v-card-item>

            <template v-if="!checkedOut">
                <v-card-text>
                    <div v-for="book in this.$props.selectedBooks" :key="book" class="pt-4">
                        <div class="text-subtitle-1 font-weight-bold">{{ book['Book Title'] }}
                        </div>
                        <div class="text-caption">by {{ book.Author }}</div>
                    </div>
                </v-card-text>
                <v-card-text>
                    <div class="pb-6">
                        Your books are due back two weeks after you reserve them. Happy Reading!
                    </div>
                </v-card-text>
                <v-card-actions>
                    <div class="w-100 d-flex justify-end pr-4">
                        <v-btn variant="flat" @click="$emit('closeCheckoutModal')">Cancel</v-btn>
                        <v-btn variant="flat" color="#2A5783" @click="checkedOut = true">Checkout</v-btn>
                    </div>
                </v-card-actions>
            </template>
            <template v-else>
                <v-card-text class="py-6">
                    <div class="d-flex justify-center flex-column flex-grow-1 w-100 pt-4">

                        <div class="text-subtitle-2 d-flex justify-center pt-4">Your order number is:</div>
                        <div class="d-flex justify-center text-h3">123456</div>

                    </div>
                </v-card-text>
                <v-card-actions>
                    <div class="d-flex justify-end w-100 pt-8">
                        <v-btn variant="flat" @click="$emit('closeCheckoutModal'); checkedOut = false;">Close</v-btn>
                    </div>
                </v-card-actions>
            </template>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: {
        showCheckoutModal: { type: Boolean, default: () => true },
        selectedBooks: { type: Array, default: () => [] }
    },
    data: () => ({
        showCheckoutDialog: true,
        checkedOut: false
    })
}
</script>

<style></style>