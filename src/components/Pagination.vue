<template>
	<div>
    <div class="pagination">
      <ul class="pages-list">
        <li class="pages-list__item" v-for="page in pages">
          <a :href="'#page-' + page" v-on:click="$emit('click', page)">{{page}}</a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
	export default {
		props: {
			perPage: Number,
			maxResults: Number,
		},
		data() {
			return {
				pages: [],
			}
		},
		created() {
			this.setPages();
		},
		methods: {
			setPages() {
        var pageCount = Math.ceil(this.maxResults / this.perPage);
        for (let i = 1; i <= pageCount; i++) {
          this.pages.push(i);
        } 
      },
		}
	}
</script>

<style scoped lang="scss">
	.pagination {
    display: flex;  
    align-items: center;
    justify-content: center;
    width: 88%;
    margin: auto;
    padding: 30px 0 10px;
  }
  .pages-list {
    list-style: none;
    display: flex;
    justify-content: center;
  }

  .pages-list__item {
      list-style: none;
      float: left;
      margin: 0 10px;
  }
</style>
