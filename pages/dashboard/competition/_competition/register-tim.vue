<template>
  <DashboardWrapper>
    <v-container>
      <div class="px-12 mx-12">
        <h5 class="mt-4 title font-weight-black">
          {{ title }}
        </h5>
        <v-text-field v-model="team" label="Team Name" />
        <v-text-field v-model="school" label="Institute / School" />
        <div class="my-2">
        <v-btn class="my-5 primary subtitle-2 text-none px-5 font-weight-bold" style="border-radius: 50px">
          Save Team
        </v-btn>
        </div>
      </div>
    </v-container>
  </DashboardWrapper>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import DashboardWrapper from '~/components/partials/Dashboard/DashboardWrapper.vue';
@Component({
  components: { DashboardWrapper }
})
export default class Competitive extends Vue {
  team: string = '';
  school: string = '';
  id: string = '';
  title: string = '';

  mounted() {
    this.id = this.$route.params.competition;
    var i;
    var temp = this.id.split("-");
    for (i = 0; i < temp.length; i++) {
      this.title += this.jsUcfirst(temp[i]);
      if (i != temp.length - 1) {
        this.title += ' ';
      }
    }
  }
  head() {
    return {
      title: 'Pendaftaran' + this.title
    };
  }
  jsUcfirst(string) 
  {
      return string.charAt(0).toUpperCase() + string.slice(1);
  }
}
</script>

<style scoped>
</style>