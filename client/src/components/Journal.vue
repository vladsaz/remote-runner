<template>
<div class="container">
  <div class="row">
    <div class="col-12">

      <vuetable-pagination-info ref="paginationInfoTop"></vuetable-pagination-info>
      <vuetable-pagination ref="paginationTop" @vuetable-pagination:change-page="onChangePage"></vuetable-pagination>

      <vuetable ref="vuetable"
        api-url="https://vuetable.ratiw.net/api/users"
        :fields="fields"
        pagination-path=""
        :per-page="10"
        :multi-sort="true"
        :sort-order="sortOrder"
        @vuetable:pagination-data="onPaginationData">
      </vuetable>

      <vuetable-pagination-info ref="paginationInfo"></vuetable-pagination-info>
      <vuetable-pagination ref="pagination" @vuetable-pagination:change-page="onChangePage"></vuetable-pagination>

    </div>
  </div>
</div>
</template>


<script>
import Vue from "vue";
import accounting from 'accounting';
import moment from 'moment';
import Vuetable from "vuetable-2/src/components/Vuetable";
import VuetablePagination from "vuetable-2/src/components/VuetablePagination";
import VuetablePaginationInfo from "vuetable-2/src/components/VuetablePaginationInfo";

let myJournal = {
  components: {
    Vuetable,
    VuetablePagination,
    VuetablePaginationInfo
  },
  data () {
    return {
      fields: [
        {
          name: 'name',
          sortField: 'name',
        }, 
        {
          name: 'email',
          sortField: 'email'
        },
        {
          name: 'birthdate',
          sortField: 'birthdate',
          titleClass: 'center aligned',
          dataClass: 'center aligned',
          callback: 'formatDate|DD-MM-YYYY'
        },
        {
          name: 'nickname',
          sortField: 'nickname',
          callback: 'allcap'
        },
        {
          name: 'gender',
          sortField: 'gender',
          titleClass: 'center aligned',
          dataClass: 'center aligned',
          callback: 'genderLabel'
        },
        {
          name: 'salary',
          sortField: 'salary',
          titleClass: 'center aligned',
          dataClass: 'right aligned',
          callback: 'formatNumber'
        }
      ],
      sortOrder: [
        {
          field: 'email',
          sortField: 'email',
          direction: 'asc'
        }
      ]
    }
  },
  methods: {
  getData () {
      this.$http.get('http://rr-test-vlada.herokuapp.com/commands').then(response => {

        // get body data
        console.log(response.body);

      }, response => {
        // error callback
      });
  },
    allcap (value) {
      return value.toUpperCase()
    },
    genderLabel (value) {
      return value === 'M'
        ? '<span class="ui teal label"><i class="large man icon"></i>Male</span>'
        : '<span class="ui pink label"><i class="large woman icon"></i>Female</span>'
    },
    formatNumber (value) {
      return accounting.formatNumber(value, 2)
    },
    formatDate (value, fmt = 'D MMM YYYY') {
      return (value == null)
        ? ''
        : moment(value, 'YYYY-MM-DD').format(fmt)
    },
    onPaginationData (paginationData) {
      this.$refs.paginationTop.setPaginationData(paginationData)
      this.$refs.paginationInfoTop.setPaginationData(paginationData)
      this.$refs.pagination.setPaginationData(paginationData)
      this.$refs.paginationInfo.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    }
  }
};

Vue.component("journal", myJournal);
export default myJournal;
</script>

<style media="screen">

</style>
