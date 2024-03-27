<template>
    <v-dialog v-model="this.$props.showModal" maxWidth="725px" style="overflow: hidden" persistent>
        <template v-slot:default="{ isActive }">
            <v-card scrollable="false">
                <v-card-item>
                    <v-card-title>
                        <div class="d-flex justify-space-between">
                            <div>Select Genre</div>
                            <div>
                                <v-icon icon="mdi-close" color="#2A5783" style="cursor:pointer"
                                    @click="$emit('closeFilterModal')"></v-icon>
                            </div>

                        </div>

                    </v-card-title>
                    <v-card-subtitle class="mt-0">Drag Your Selection(s)</v-card-subtitle>
                </v-card-item>

                <v-card-text class="pt-4 pb-0 mb-0">
                    <v-row>
                        <v-col cols="5">
                            <div class="text-caption">Available:</div>
                        </v-col>

                        <v-col cols="7">
                            <div class="text-caption">Selected:</div>
                        </v-col>
                    </v-row>
                </v-card-text>

                <v-card-text>
                    <v-row>
                        <v-col cols="5">
                            <div class="d-flex">
                                <div>
                                    <v-slide class="one-per-row">
                                        <v-chip-group column>
                                            <draggable :list="genres"
                                                :group="{ name: 'genres', put: false, pull: 'clone' }" :itemKey="item">
                                                <template #item="{ element }">
                                                    <v-chip class="filter-pill" variant="elevated" density="comfortable"
                                                        size="large">
                                                        <v-icon icon="mdi-bookshelf" size="s" start></v-icon>

                                                        {{ element }}
                                                    </v-chip>

                                                </template>
                                            </draggable>
                                        </v-chip-group>
                                    </v-slide>
                                </div>
                            </div>
                        </v-col>
                        <v-col cols="7">

                            <div class="d-flex" style="min-height: 100%">
                                <div style="background-color: lightgray; border-radius: 10px;" class="flex-grow-1 px-2">
                                    <v-slide class="one-per-row">
                                        <div style="display: float-right; position: relative; top: 4px; float: right; cursor: pointer;"
                                            v-if="selectedGenres.length > 0" @click="clearFilters()">
                                            <v-icon icon="mdi-close"></v-icon>
                                        </div>
                                        <v-chip-group column>
                                            <draggable class="list-group w-100" :list="selectedGenres" group="genres"
                                                :itemKey="item" style="min-height: 100px">
                                                <template #item="{ element }">
                                                    <v-chip class="filter-pill-selected" variant="elevated"
                                                        density="comfortable" size="large">
                                                        <v-icon icon="mdi-bookshelf" size="s" start></v-icon>
                                                        {{ element }}
                                                    </v-chip>

                                                </template>
                                            </draggable>
                                        </v-chip-group>
                                    </v-slide>
                                </div>
                            </div>
                        </v-col>
                    </v-row>
                </v-card-text>
                <v-card-actions class="py-4">
                    <div class="w-100 d-flex justify-end pr-4">
                        <v-btn variant="flat"
                            @click="$emit('clearFilters'); $emit('closeFilterModal'); clearFilters()">Clear
                            Selections</v-btn>
                        <v-btn color="#2A5783" variant="flat"
                            @click="$emit('filtersSelected', selectedGenres); $emit('closeFilterModal')">Apply
                            Filters</v-btn>
                    </div>
                </v-card-actions>
            </v-card>
        </template>
    </v-dialog>
</template>
<script>
import { reactive } from 'vue'
import draggable from 'vuedraggable'
import _ from 'lodash'

export default {
    props: { showModal: { type: Boolean, default: () => false } },
    components: {
        draggable
    },
    data: () => ({
        selectedGenres: [],
        genres: ['Adventure', 'Comedy', 'Fantasy', 'Fiction', 'Horror', 'Mystery', 'Non-Fiction', 'Romance', 'Sci-Fi', 'Thriller', 'YA'],
        showDialog: false
    }),
    computed: {},
    methods: {
        clearFilters() {
            this.selectedGenres = []
            this.$emit('clearVizFilters')


        }
    },
    watch: {}
}
</script>
<style>
.filter-pill {
    background-color: #e0e0e0 !important;
    color: #424242 !important
}

.filter-pill-selected {
    background-color: #2A5783 !important;
}

.one-per-row .v-chip-group--column .v-slide-group__content {
    display: block
}

.one-per-row .v-chip-group--column .v-chip {
    float: left;
    clear: both;
}
</style>