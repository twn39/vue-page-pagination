<template>
  <div class="page-pagination" v-if="show">
    <span @click="prePage">{{previous}}</span>
    <a href="" @click.prevent="changePage(item)" v-for="item in preRange">
      {{item}}
    </a>
    <a class="active">{{info.current_page}}</a>
    <a href="" @click.prevent="changePage(item)" v-for="item in nextRange">{{item}}</a>
    <span @click="nextPage">{{next}}</span>
  </div>
</template>

<script>
  import Paginator from 'page-pagination';

  export default {
    props: ['total', 'page', 'perPage', 'segment', 'previous', 'next'],
    data() {
      return {
        paginator: {},
        info: {},
      };
    },
    created() {
      this.paginator = new Paginator(this.perPage, ((this.segment * 2) + 1));
      this.info = this.paginator.build(this.total, this.page);
    },
    computed: {
      show() {
        return (!(this.info.first_page === 1 && this.info.last_page === 1));
      },
      preRange() {
        const range = [];
        for (let i = this.info.first_page; i < this.info.current_page; i++) {
          range.push(i);
        }
        return range;
      },
      nextRange() {
        const range = [];
        for (let i = this.info.current_page + 1; i <= this.info.last_page; i++) {
          range.push(i);
        }
        return range;
      },
    },
    methods: {
      changePage(page) {
        this.info = this.paginator.build(this.total, page);
        this.$emit('change', page);
      },
      prePage() {
        const prePage = this.info.current_page - 1;
        this.info = this.paginator.build(this.total, prePage);
        this.$emit('change', prePage);
      },
      nextPage() {
        const nextPage = this.info.current_page + 1;
        this.info = this.paginator.build(this.total, nextPage);
        this.$emit('change', nextPage);
      },
    },
  };
</script>
