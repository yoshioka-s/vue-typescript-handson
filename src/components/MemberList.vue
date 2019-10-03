<template>
  <ul>
    <li v-for="(member, i) in members" v-bind:key="i">
      <i class="fa" v-bind:class="ICONS[member.sex]"></i>
      {{ member.name }}
    </li>
  </ul>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { sexString, MemberInterface } from '@/interfaces/Member';

@Component
export default class MemberList extends Vue {
  @Prop({ type: Array, default: [] })
  public members!: MemberInterface[];

  public ICONS = {
    男: 'fa-mars',
    女: 'fa-venus',
  };
}
</script>

<style lang="stylus">
.fa-mars {
  color: #03328b;
}

.fa-venus {
  color: #eb90c8;
}
</style>