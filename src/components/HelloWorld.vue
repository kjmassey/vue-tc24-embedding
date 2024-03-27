<template>
  <NavBar class="pb-4" :booksToCheckout="formattedSelectedMarks && formattedSelectedMarks"
    @show-checkout-modal="showCheckoutModal = true" />
  <FilterChips :showModal="showFilterModal" @close-filter-modal="showFilterModal = false"
    @filters-selected="updateFilters" @clear-filters="revertViz" />
  <div class="d-flex w-100 justify-space-between">
    <div class="w-100">
      <FilterToggle @open-filter-modal="showFilterModal = true"
        :countSelectedItems="formattedSelectedMarks && formattedSelectedMarks.length"
        @clear-selected-books-set="revertViz" @open-info-dialog="showInfoDialog = true" />
    </div>

    <div class="d-flex justify-center">
      <tableau-viz src="https://10ax.online.tableau.com/t/kjmdev797388/views/TCEmbedding-Books/BooksDashboard"
        device="desktop" hide-tabs toolbar="hidden">
      </tableau-viz>
    </div>

    <div class="w-100"></div>
  </div>
  <CheckOutModal :showCheckoutModal="showCheckoutModal" @close-checkout-modal="showCheckoutModal = false"
    :selectedBooks="formattedSelectedMarks && formattedSelectedMarks" />
  <AboutModal @close-info-dialog="showInfoDialog = false" :showInfoDialog="showInfoDialog" />
</template>
<script>
import draggable from 'vuedraggable'
import _ from 'lodash'
import SelectState from './SelectState.vue'
import NavBar from './NavBar.vue'
import FilterChips from './FilterChips.vue'
import FilterToggle from './FilterToggle.vue'
import CheckOutModal from './CheckOutModal.vue'
import AboutModal from './AboutModal.vue'

export default {
  name: "two-lists",
  display: "Two Lists",
  order: 1,
  components: {
    draggable,
    SelectState,
    NavBar,
    FilterChips,
    FilterToggle,
    CheckOutModal,
    AboutModal
  },
  data() {
    return {
      embeddedViz: null,
      wsFilters: [],
      sortedFilters: [],
      selected: [],
      showFilterModal: false,
      selectedBooks: [],
      showCheckoutModal: false,
      showInfoDialog: true
    };
  },
  computed: {
    formattedFilters() {
      // Turn the complex wsFilters array into a simple array of the available filter values
      let filterVals = [];

      this.wsFilters &&
        this.wsFilters[0]._appliedValues.forEach((e) => {
          filterVals.push(e._value);
        });

      return filterVals;
    },
    formattedColNames() {
      let cols = this.selectedBooks._columns;

      return cols.map((e) => e._fieldName);
    },
    formattedSelectedMarks() {
      // Transform the selected marks data into a readable/tabular array of objects
      const marksData = [];

      if (this.selectedBooks.length == 0) {
        return []
      }

      this.selectedBooks._data.forEach((e) => {
        if (Array.isArray(e)) {
          marksData.push(e);
        }
      });

      const formattedMarks = [];

      for (let mark of marksData) {
        const formattedObj = {};

        for (let [index, value] of mark.entries()) {
          formattedObj[this.formattedColNames[index]] = value._value;
        }
        formattedMarks.push(formattedObj);
      }

      return formattedMarks;
    },
  },
  methods: {
    add: function () {
      this.list.push({ name: "Juan" });
    },
    replace: function () {
      this.list = [{ name: "Edgard" }];
    },
    clone: function (el) {
      return {
        name: el.name + " cloned"
      };
    },
    log: function (evt) {
      window.console.log(evt);
    },
    updateList2(e) {
      console.log(e)
    },
    resetLists() {
      this.list1 = _.cloneDeep(this.list1_orig)
      this.list2 = _.cloneDeep(this.list2_orig)
      this.list3 = []
    },
    async initViz() {
      // This is required when the page loads and any time the embedded viz changes in order for it to be interactive!

      // Set the <tableau-viz> component to be our embedded viz for interactivity -- this all works because of the import added to index.html
      const viz = document.querySelector("tableau-viz");

      this.embeddedViz = viz;

      // Add an event listener to verify the viz becomes interactive
      await new Promise((resolve) => {
        this.embeddedViz.addEventListener("firstinteractive", () => {
          console.log("Viz is interactive!");
          resolve();
        });
      });

      await new Promise((resolve) => {
        this.embeddedViz.addEventListener("markselectionchanged", () => {
          this.getSelectedMarks()
          resolve()
        })
      })
    },
    // async getFilters() {
    //   // Get details about the filters for the activated sheet
    //   const viz = this.embeddedViz;

    //   const worksheet = await viz.workbook.activateSheetAsync(
    //     'Scatter'
    //   );

    //   this.wsFilters = await worksheet.getFiltersAsync();
    // },
    async updateFilters(e) {
      // Update a filter in the activated sheet by passing its name and an array of values
      const viz = this.embeddedViz;
      const filterArr = JSON.parse(JSON.stringify(e))

      if (filterArr.length == 0) {
        this.resetFilters()
        return
      }

      const dashboard = await viz.workbook.activateSheetAsync('Books Dashboard');

      dashboard.worksheets.forEach(async x => {
        if (x.name == 'Books Table') {
          console.log('FILTERS TO APPLY: ', filterArr)
          await x.applyFilterAsync('Super Genre', filterArr, 'replace')
        }
      })

      // Stringify and then re-parse the selectedFilters array
      // This is necessary because of reactivity in Vue, i.e. it forces a value from what is otherwise a reference to a resolved Promise
      // const filterArr = JSON.parse(JSON.stringify(this.selectedFilters));

      // var filterVals = e.map(e => e)
      // console.log('FILTER VALS: ', filterVals)

      // await worksheet.applyFilterAsync("Book Title", filterVals, "replace");
    },
    async getSelectedMarks() {
      const viz = this.embeddedViz
      const dashboard = await viz.workbook.activateSheetAsync('Books Dashboard')

      dashboard.worksheets.forEach(async w => {
        if (w.name == 'Books Table') {
          const selectedMarks = await w.getSelectedMarksAsync()

          this.selectedBooks = selectedMarks.data[0]
        }
      })
    },
    async updateFiltersFromEmit(selections) {
      // Update a filter in the activated sheet by passing its name and an array of values
      const viz = this.embeddedViz;

      const worksheet = await viz.workbook.activateSheetAsync('Scatter');

      // Stringify and then re-parse the selectedFilters array
      // This is necessary because of reactivity in Vue, i.e. it forces a value from what is otherwise a reference to a resolved Promise
      // const filterArr = JSON.parse(JSON.stringify(this.selectedFilters));

      var filterVals = selections.map(e => e)
      console.log('FILTER VALS: ', filterVals)

      await worksheet.applyFilterAsync("State/Province", filterVals, "replace");
    },
    async resetFilters() {
      // Update a filter in the activated sheet by passing its name and an array of values
      const viz = this.embeddedViz;

      const dashboard = await viz.workbook.activateSheetAsync('Books Dashboard');

      dashboard.worksheets.forEach(async w => {
        if (w.name == 'Books Table') {
          await w.clearFilterAsync("Super Genre")
        }
      })
    },
    async revertViz() {
      const viz = this.embeddedViz
      this.selectedBooks = []

      await viz.revertAllAsync()
    },
    changeState(e) {
      this.updateFiltersFromEmit(e)
    }
  },
  watch: {

  },
  async mounted() {
    await this.initViz()
    // await this.getFilters()

    // this.sortedFilters = _(this.formattedFilters).sort()
  },
};
</script>