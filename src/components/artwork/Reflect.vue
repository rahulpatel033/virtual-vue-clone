<template>
  <li class="dropdown">
  <a href="javascript:void(0);" data-toggle="dropdown" data-hover="tooltip" data-placement="left" title="Reflect" @click="isImageText">
        <i class="icon-Flip"></i>
    </a>
    <div class="dropdown-menu" v-if="isWorkSelected">
      <h2>Reflect</h2>
        <div class="reflect-but">
          <a href="javascript://" @click="addFlipFlop('flop')" ><span class="icon-Horizontal-Flip"></span></a>
            <a href="javascript://" @click="addFlipFlop('flip')" ><span class="icon-Vertical-Flip"></span></a>
        </div>
    </div>
</li>
</template>
<script>
import { mapGetters } from 'vuex'

export default {
  name: 'reflect',
  data () {
    return {
      open: true,
      flip: 0,
      flop: 0,
    }
  },
  computed: {
    ...mapGetters({
      cordinates: 'getImageCordinates',
      selecteArea: 'getIsSelectedArea'
    }),
    isWorkSelected: function() {
      return (this.selecteArea==null) ? false : true
    }
  },
  methods: {
    isImageText() {
      if(this.isWorkSelected === false){
        alert("Please select a Image/Text")
        return false
      }
    },
    addFlipFlop(type){

      if(type=='flip'){
        if(this.flip==0) this.flip = 1
        else this.flip = 0
      }else{
        if(this.flop==0) this.flop = 1
        else this.flop = 0
      }
      this.setFlipFlop(type)
    },
    setFlipFlop(type){
      let selected = this.selecteArea.value
      let selectedKey = this.selecteArea.key
      selected = parseInt(selected)-1

      let newcordinates = this.cordinates

      if(type=='flip'){
        if(selectedKey=='image') newcordinates.flip[selected].value = this.flip;
        else newcordinates.text_flip[selected].value = this.flip;
      }else{
        if(selectedKey=='image') newcordinates.flop[selected].value = this.flop;
        else newcordinates.text_flop[selected].value = this.flop;
      }

      this.$store.commit('setImageCordinates', { cordinates:newcordinates } )
      return this.$store.dispatch('generateSequence',this.cordinates)
    }
  }
}
</script>
