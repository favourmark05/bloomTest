<template>
  <div class="main">
    <header>
      <div class="col"></div>
      <div class="col">
        <h4> {{title}} </h4>
      </div>
      <div class="col filter">
        <button class="color" @click="showColour()">Colour</button>
        <button class="shape" @click="showShape()">Shape</button>
      </div>
    </header>
    <div class="filters" v-if="colour || shape">
      <div class="heading" v-if="colour">
        <div>
          <h3>Colour</h3>
          <button class="close" @click="closeFilter()">X</button>
        </div>
        <label for="" v-for="color in colors" :key="color">
          <span>{{  color  }}</span><input type="checkbox"
            v-model="glass_variant_frame_variant_colour_tag_configuration_names" multiple id="" :value="color" />
        </label>
      </div>
      <div class="heading" v-if="shape">
        <div>
          <h3>Shape</h3>
          <button class="close" @click="closeFilter()">X</button>
        </div>
        <label for="" v-for="shape in shapes" :key="shape">
          <span>{{  shape  }}</span><input type="checkbox"
            v-model="glass_variant_frame_variant_frame_tag_configuration_names" multiple :value="shape" />
        </label>
      </div>
    </div>
    <div class="content">
      <div v-for="collection in glasses" :key="collection.id">
        <div class="card">
          <h3 class="name"> {{  collection.name  }}</h3>
          <img class="img-fluid" :src="collection.glass_variants[0].media[0].url" alt="" srcset="">
        </div>
      </div>
    </div>
    <div class="load" v-show="loader">
        <img src="../assets/loader.gif" alt="" srcset="" class="loader">
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      pagenumber: 2,
      loader: false,
      colour: false,
      shape: false,
      glasses: [],
      total_collections: 0,
      title: '',
      colors: ['black', 'tortoise', 'coloured', 'crystal', 'dark', 'bright'],
      shapes: ['square', 'rectangle', 'round', 'cat-eye'],
      glass_variant_frame_variant_colour_tag_configuration_names: [],
      glass_variant_frame_variant_frame_tag_configuration_names: [],
      queryString: '',
      config: '',
      params: {
        params: {
          "page['limit']": 12,
          "page['number']": 1
        }
      }

    }
  },
  watch: {
    glass_variant_frame_variant_colour_tag_configuration_names (
      newValue,
      oldValue
    ) {
      newValue.map((name, i) => {
        this.params.params[
          'filters[glass_variant_frame_variant_colour_tag_configuration_names][]'
        ] = name
      })
      this.getGlasses(this.config)
    },
    glass_variant_frame_variant_frame_tag_configuration_names (
      newValue,
      oldValue
    ) {
      newValue.map((name) => {
        this.params.params['filters[glass_variant_frame_variant_frame_tag_configuration_names][]'] = name
      })
      this.getGlasses(this.config)
    }
  },
  methods: {
    showColour () {
      this.colour = true
      this.shape = false
    },
    showShape () {
      this.colour = false
      this.shape = true
    },
    closeFilter () {
      this.colour = false
      this.shape = false
    },
    getGlasses (collection) {
      this.loader = true
      this.axios
        .get(
          `https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections/${collection}/glasses?page[number]=1&page[limit]=12`, this.params
        )
        .then((res) => {
          this.loader = false
          this.glasses = res.data.glasses
          this.total_collections = res.data.meta.total_count
        })
    },
    getNextGlasses () {
      const collection = this.$route.params.config
      if (this.glasses.length <= this.total_collections - 1 && !this.loader) {
        if (window.scrollY + window.innerHeight >= document.body.scrollHeight - 50) {
          this.loader = true
          this.axios
            .get(
          `https://staging-api.bloobloom.com/user/v1/sales_channels/website/collections/${collection}/glasses?page[number]=${this.pagenumber++}&page[limit]=12`
            )
            .then((res) => {
              this.loader = false
              const newPost = res.data.glasses
              for (let i = 0; i < newPost.length; i++) {
                this.glasses.push(newPost[i])
                this.loader = false
              }
            }).catch(err => {
              this.loader = false
              console.log(err)
            })
        }
      }
    }
  },
  mounted () {
    this.config = this.$route.params.config
    this.title = this.config.split('-').map(c => c.toUpperCase()).join(' ')
    this.getGlasses(this.config)
    window.addEventListener('scroll', this.getNextGlasses)
  }
}
</script>

<style scoped>
.main {
  padding-top: 50px;
}

h3 {
  font-size: 100%;
}

header {
  border: 2px solid rgba(51, 50, 50, 0.438);
  display: grid;
  grid-template-columns: 33.33% 33.33% 33.33%;
  text-align: center;
}
.color, .shape {
  background-color: #ffff !important;
}
.col {
  border-right: 2px solid gray;
}
.load{
  display: flex;
}
.loader{
  margin: auto;
  width:100px
}
.col>h4 {
  padding: 10px 0;
}
label {
  margin: 20px;
  display: inline-flex;
}
span {
  padding-right: 10px;
}
.heading {
  text-align: center;
}
.filter {
  display: flex;
}
.heading > div {
  display: flex;
}
.heading > div > h3 {
  margin: auto;
}
.close {
  background-color: gray;
  color: white;
  margin: 20px;
  padding: 5px 10px;
  border-radius: 100%;
}
select {
  padding: 10px 20px;
  -webkit-appearance: none;
}

.content {
  display: grid;
  grid-template-columns: auto auto auto;
}

.card {
  border: 1px solid rgb(100, 98, 98);
  width: 100%;
}

.name {
  position: absolute;
  padding-left: 10%;
}
button {
  font-size: 1vw;
  padding: 3px 22px;
  cursor: pointer;
  border: none;
  border-right: 2px solid gray !important;
}

img {
  width: 100%;
}

@media only screen and (max-width: 900px) {
  .content {
    grid-template-columns: auto auto;
  }
}

@media only screen and (max-width: 500px) {
  .content {
    grid-template-columns: auto;
  }
}
</style>
