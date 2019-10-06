<template>
  <div class="row">
    <div class="col-sm-16 row">
      <h2 class="col-sm-12">メンバー {{ members.length }}人</h2>
    </div>
    <div class="col-sm-8 row">
      <member-list class="col-sm-6" :members="boys"></member-list>
    </div>
    <div class="col-sm-8 row">
      <member-list class="col-sm-6" :members="girls"></member-list>
    </div>
  </div>
</template>

<script lang="ts">
import _ from 'lodash';
import { Component, Vue } from 'vue-property-decorator';
import { MemberInterface } from '@/interfaces/Member';
import MemberList from '@/components/MemberList.vue';

@Component({
  components: {
    MemberList,
  },
})
export default class MemberLists extends Vue {
  public members: MemberInterface[] = [];
  get sortedMembers(): MemberInterface[] {
    // membersをordering順に並べて返す
    return _.sortBy(this.members, 'ordering');
  }
  get girls(): MemberInterface[] {
    return _.filter(this.sortedMembers, { sex: '女' });
  }
  get boys(): MemberInterface[] {
    return _.filter(this.sortedMembers, { sex: '男' });
  }
}
</script>