<template>
  <div id="app" class="container">
    <nav class="navbar navbar-light bg-light navbar-expand-lg">
      <a href="/" class="navbar-brand">
        <h1>席替え番長</h1>
      </a>
    </nav>
    <div class="row page">
      <div class="col-sm-8 row">
        <h2 class="col-sm-12">メンバー {{ members.length }}人</h2>
        <member-list class="col-sm-6" :members="girls"></member-list>
        <member-list class="col-sm-6" :members="boys"></member-list>
        <button v-if="members.length > 0" v-on:click="shuffle" class="btn btn-success">席替え</button>
      </div>

      <div class="col-sm-4">
        <h2>メンバー追加</h2>
        <member-form @addMember="save"></member-form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import _ from 'lodash';
import { MemberInterface } from '@/interfaces/Member';
import MemberList from '@/components/MemberList.vue';
import MemberForm from '@/components/MemberForm.vue';

@Component({
  components: {
    MemberList,
    MemberForm,
  },
})
export default class App extends Vue {
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

  public save(member: MemberInterface): void {
    // orderingで席順を管理。新メンバーは最後尾
    const newMember = Object.assign({}, member, {
      ordering: this.members.length,
    });
    this.members.push(newMember);
  }
  public shuffle(): void {
    _(this.members)
      // this.membersの配列順をシャッフル
      .shuffle()
      // orderingをシャッフル後の席順に更新
      .each((member, i) => (member.ordering = i));
  }
}
</script>

<style lang="stylus">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 50px;
  color: #2c3e50;
}

.page {
  min-height: 300px;
}
</style>
