<template>
  <div class="text-left p-3">
      <div v-if="targetPublication == null">
        <div class="header-back">
          <span class="icon-pictos-bridgeback ico-back color-orange" @click="goBack()">
            <span class="l l1"></span>
            <span class="l l2"></span>
          </span>
          <h2>{{$t('use-case')}}</h2>  
        </div>
        <vuescroll class="vueScroll">
          <ul class="list-group list-item" v-if="solutionsActive.length > 0">
            <li class="list-group-item list-group-item-action" @click.prevent="setTargetPub(data)"
                v-for="(data, i) in solutionsActive" :key="i">
                <div class="text-muted small">{{$t('last-update')}} : {{getDate(data.modified)}}</div>

                <h3 class="mb-2">{{data.acf.titre_de_la_solution}}</h3>

                <div v-if="data.acf.pays_enreg_structure"
                  class="d-flex align-items-center">
                  <span class="icon-pictos-bridgepictos_Plan-de-travail-1 mr-2"></span>
                  <span class="small mr-1">
                  {{data.acf.pays_enreg_structure}} 
                  </span>
                </div>
                
                <div v-if="data._embedded['wp:term'][4].length > 0"
                  class="d-flex align-items-baseline mt-2">
                  <span class="icon-pictos-bridgepictos-02 mr-2"></span>
                  <div>
                    <div class="small" v-html="data.acf.categorie_solution"></div>
                    <div style="margin-top:-5px">
                      <span class="text-pays mr-1 mt-n1" v-for="(item, id) in data._embedded['wp:term'][4]" :key="'pays'+id">
                        <span v-if="id != 0" class="small"> | </span> 
                        <span class="small d-inline" v-html="item.name"></span> 
                      </span>
                    </div>
                  </div>
                </div>

                <p class="mt-3" v-html="resume(data.acf.description_solution)"></p>
                <a href="#!" class="more" @click.prevent="setTargetPub(data)">
                  {{$t('more')}} <span class="icon-pictos-bridgego color-orange"></span>
                </a>
            </li>
          </ul>
          <p v-else class="small text-center text-muted p-4">
            {{$t('use-case-not-found')}} 
          </p>
        </vuescroll>
      </div>
      <Publication :pub="targetPublication" v-else @back="resetPub()"></Publication>
  </div>

</template>

<script>
import { mapState, mapActions } from "vuex" ;
import Publication from "./Publication"
import vuescroll from 'vuescroll';

export default {
  components : {
    Publication,
    vuescroll
  } ,
  data(){
    return {
      targetPublication : null
    }
  },
  mounted() {
  },
  computed: {
    ...mapState(['solutionsActive'])
  },
	methods: {
    ...mapActions(['setMenu']),
    getDate(datePub){
        const date = new Date(datePub)
        let month = (date.getMonth() + 1)
        month = month.length > 1 ? month : '0' + month
        return date.getDate() + "/" + month + "/" + date.getFullYear()
    },
    goBack(){
      this.setMenu(1)
    },
    setTargetPub(data){
      this.targetPublication = data
    },
    resetPub(){
      this.targetPublication = null
    },
    resume(slug){
      return slug.substr(0,130) + '...'
    }
  },
  watch :{
    solutionsActive(){
      this.resetPub()
    }
  }
}
</script>


<style lang="scss" scoped>
  @import "../assets/sass/_variables.scss";
  .list-group-item{
    cursor: pointer;
    .more{
      display: flex;
      align-items: center;
      color: $orange ;
    }
  }
  .list-item{
    margin-bottom: 20px;
    margin-left: 0px;
    li{
      margin: 0px;
    }
  }
  @media screen  and (min-width: 992px){
    .vueScroll{
      height:400px !important;
      background-color: white;
    }
  }
  .header-back h2{
    transform: scale(1);
    @media (min-width: 992px){
      transform: scale(0);
    }
  }
</style>