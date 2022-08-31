<template>
  <div>
    <div class="navbar">
      <div class="content">
        <div class="menu grid-item" ref="main_nav">
          <span>MENU</span>
          <div class="side-menu" ref="side_menu">
            <li v-for="collection in collections" :key="collection.id">
              <a :href="'/collection/'+collection.configuration_name">{{collection.name}}</a>
            </li>
            <li>
              HOME TRY ON
            </li>
            <li>PAIR FOR PAIR</li>
          </div>
        </div>
        <div class="grid-item">
          <router-link to="/">
            <img src="https://i.pinimg.com/236x/a0/23/73/a0237397bfeabd8d9c8a89031d7b3921--art-flowers-light-blue.jpg"
            alt="logo" srcset="" width="30px" height="30px">
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      collections: []
    }
  },
  methods: {
    getCollections () {
      this.axios
        .get(
          'https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections'
        )
        .then((res) => {
          this.collections = res.data.collections
          console.log(this.collections)
        })
    }
  },
  mounted () {
    this.getCollections()
    console.log(this.$refs, 'reds')
    this.$refs.main_nav.addEventListener('mouseover', () => {
      this.$refs.side_menu.classList.add('active')
    })
    this.$refs.side_menu.addEventListener('mouseout', () => {
      this.$refs.side_menu.classList.remove('active')
    })
  }
}
</script>

<style scoped>
.content {
  display: grid;
  grid-template-columns: 10% 90%;
  align-content: center;
  /* position: relative; */
}

.navbar {
  border-bottom: 1px solid gray;
  position: fixed;
  padding: 10px 20px;
  width: 100%;
  z-index: 100;
  background-color: white;
}

.side-menu {
  position: absolute;
  background-color: white;
  top: 100%;
  left: 0;
  z-index: 1000;
  /* padding: 40px; */
  height: 100vh;
  border-top: 2px solid gray;
  border-right: 2px solid gray;
  display: none;
  cursor: pointer;
}
.active{
  display: block;
}
li {
  padding: 10px 30px;
  border-bottom: 2px solid gray;
  list-style-type: none;
}
a {
  text-decoration: none;
  list-style-type: none;
  color: black;
}
li:hover, a:hover{
  background-color: black;
  color: white;
}
.grid-item {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
