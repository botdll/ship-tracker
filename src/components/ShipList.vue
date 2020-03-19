<template>
  <div>
   
    <b-form inline>
      <b-row>
        <b-col class="mb-3" lg="12" xl="5">
          <label class="sr-only" for="mmsi-number">MMSI</label>
          <b-input-group prepend="MMSI:" class="mb-2 mr-sm-2 mb-sm-0">
            <b-input id="mmsi-number" v-model="form.mmsi" type="text" placeholder="Enter 9 digit code" maxlength="9" required></b-input>
          </b-input-group>
        </b-col>

      <b-col class="mb-3" lg="12" xl="5">
        <label class="sr-only" for="days-range">days</label>
        <b-input-group prepend="Days" class="mb-2 mr-sm-2 mb-sm-0">
          <b-input id="days-range" v-model="form.days" type="text" placeholder="Enter days to check" required></b-input>
        </b-input-group>
      </b-col>

        <b-col class="mb-3" lg="12" xl="2">
          <b-button :disabled="form.loading" type="submit" variant="outline-primary" @click.prevent="$emit('submitted', form)">
            <b-spinner small v-if="form.loading"></b-spinner>Search
          </b-button>
        </b-col>
      </b-row>
    </b-form>
    

    <b-card no-body header="Date Location Entries" class="text-center text-primary">
      <b-list-group class="scroll-list" flush>
          <b-list-group-item
                            v-for="(ship, index) in ships"
                            :key="index"
                            @mouseover="mouseOver(index)"
                            @mouseleave="mouseLeave(index)"
                            href="#">{{ new Date(ship.TIMESTAMP).toDateString() }}</b-list-group-item>
      </b-list-group>
    </b-card>

  </div>
</template>

<script>
  export default {
    name: 'ShipList',
    props: {
      ships: Array
    },
    data() {
      return {
        form: {
          mmsi: '',
          days: '',
          loading: false
        }
      }
    },
    methods: {
      mouseOver(index) {
        this.$emit('mouse-over', index)
      },
      mouseLeave(index) {
        this.$emit('mouse-leave', index)
      }
    }
  }
</script>

<style scoped>
.scroll-list {
  overflow-y: scroll;
  height: 70vh;
}
</style>