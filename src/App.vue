<template>
  <div id="app">
    <div class="container">
      <div class="list-data">
        <div class="search">
          <div class="search__input"><input type="text" class="input" v-model="inputSearch" placeholder="Поиск"></div>
          <div v-show="inputSearch" @click="inputSearch = ''" class="btn-clear">очистить</div>
        </div>
        <div class="row-item" v-for="item in listUsersResult" :key="item.id">
          <toggle-item :item="item">
          </toggle-item>
        </div>
      </div><!--end list-data -->
      <ol class="pagination">
        <li v-for="(pageNumber, index) in pages" :key="index"><button :class="{active : page === pageNumber}" type="button" class="btn btn-sm btn-outline-secondary"  @click="getUsers(pageNumber)"> {{pageNumber}} </button></li>
      </ol>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
import toggleItem from './components/toggleItem'


export default {
  name: 'App',
  components: {
    toggleItem
  },
  data() {
    return {
      listUsers: [],
      inputSearch: '',
      isVisible: false,
      page: 1,
      perPage: 6,
      totalPage: null,
      pages: [],
    }
  },
  computed:{
    // eslint-disable-next-line vue/return-in-computed-property
    listUsersPhones(){
        return this.listUsers.map(name => ({...name, phone: this.phoneGenerator()}));
    },
    listUsersResult(){
      const inpSearch = this.inputSearch.toLowerCase();
      if (inpSearch === '' || !inpSearch) {
        return this.listUsersPhones;
      }else {
        return this.listUsersPhones.filter((inp) =>
            inp.first_name.toLowerCase().includes(inpSearch)
        )
      }
    },
  },
  created() {
    this.getUsers(this.page);
  },
  watch: {
    listUsers () {
      this.setPages(this.page);
    }
  },
  mounted() {
    window.setInterval(() => {
      this.getUsers(this.page)
    }, 6000)
    },
  methods: {
    phoneGenerator(){
      let phoneNums = Math.random().toString().slice(2,11);
      return phoneNums.slice(0,3)+"-"+phoneNums.slice(3,6)+"-"+phoneNums.slice(6);
    },
    async getUsers(page = 1){
      try {
      console.log('запрос к api каждую минуту')
      this.page = page;
      const {data: { total: totalResponse, data: usersData } } = await axios.get('https://reqres.in/api/users?page='+page+'&per_page='+this.perPage);
        this.listUsers = usersData;
        this.totalPage = totalResponse;
      } catch (error) {
        console.log(error);
      }
    },
    setPages () {
      this.pages = [];
      let numberOfPages = Math.ceil(this.totalPage / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
    },

  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.search { position: relative; display: flex;margin-bottom: 30px;}
.search__input {flex: 1 1 auto;margin-right: 10px;}
.search__input input {width: 100%;}
.btn-clear { background: #333; border-radius: 4px; padding:1px 10px;color: #fff;}
.row-item {
  border: 1px solid #888;
  padding: 5px;
  border-radius: 5px;
  margin-bottom: 15px;
}
.title {
  font-weight: bold;
  padding-bottom: 5px;
  cursor: pointer;
}
.pagination li {margin-right: 10px;}
</style>
