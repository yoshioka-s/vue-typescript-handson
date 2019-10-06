<template>
  <div id="app" class="container">
    <nav class="navbar navbar-light bg-light navbar-expand-lg">
      <a href="/" class="navbar-brand">
        <h1>席替え番長</h1>
      </a>
      <router-link :to="{name: 'form'}">Form</router-link>
      <router-link :to="{name: 'lists'}">Lists</router-link>
    </nav>
    <router-view class="page"></router-view>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import _ from 'lodash';
import { MemberInterface } from '@/interfaces/Member';
import MemberList from '@/components/MemberList.vue';

@Component({})
export default class App extends Vue {
  public members: MemberInterface[] = [];

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
