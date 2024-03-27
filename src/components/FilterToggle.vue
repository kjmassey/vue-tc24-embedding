<template>
    <div class="d-flex justify-start align-center pt-4 text-body2 w-100 text-no-wrap pl-6" style="cursor: pointer;"
        @click="$emit('openInfoDialog')">
        <v-hover>
            <template v-slot:default="{ isHovering, props }">
                <div v-bind="props">
                    <v-icon icon="mdi-information" size="28" :color="isHovering ? '#2A5783' : 'grey'"
                        v-bind="props"></v-icon>
                </div>
                <div class="pl-1" v-bind="props">
                    About the Library
                </div>
            </template>
        </v-hover>
    </div>
    <div class="d-flex justify-start align-center pt-6 text-body2 w-100 text-no-wrap pl-6" style="cursor: pointer;"
        @click="$emit('openFilterModal')">
        <v-hover>
            <template v-slot:default="{ isHovering, props }">
                <div v-bind="props">
                    <v-icon icon="mdi-filter" size="28" :color="isHovering ? '#2A5783' : 'grey'"
                        v-bind="props"></v-icon>
                </div>
                <div class="pl-1" v-bind="props">
                    Filter Selections
                </div>
            </template>
        </v-hover>
    </div>
    <div class="d-flex justify-start align-center pt-6 text-body2 w-100 text-no-wrap pl-6" style="cursor: pointer;"
        @click="$emit('clearSelectedBooksSet')" v-if="this.$props.countSelectedItems > 0">
        <v-hover>
            <template v-slot:default="{ isHovering, props }">
                <div v-bind="props">
                    <v-icon icon="mdi-close" size="28" :color="isHovering ? 'red' : 'grey'" v-bind="props"></v-icon>
                </div>
                <div class="pl-1" v-bind="props">
                    Clear Selections
                </div>
            </template>
        </v-hover>
    </div>
</template>

<script>
export default {
    props: { countSelectedItems: { type: Number, default: () => 0 } },
    data: () => ({}),

}
</script>

<style scoped></style>