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
        <ul class="col-sm-6">
          <li v-for="(member, i) in girls" v-bind:key="i">
            <i class="fa" v-bind:class="ICONS[member.sex]"></i>
            {{ member.name }}
          </li>
        </ul>
        <ul class="col-sm-6">
          <li v-for="(member, i) in boys" v-bind:key="i">
            <i class="fa" v-bind:class="ICONS[member.sex]"></i>
            {{ member.name }}
          </li>
        </ul>
        <button v-if="members.length > 0" v-on:click="shuffle" class="btn btn-success">席替え</button>
      </div>

      <div class="col-sm-4">
        <h2>メンバー追加</h2>
        <form class="member-form" v-on:submit.prevent="save">
          <div class="form-group">
            <label for="sex">性別</label>
            <br />
            <p v-for="option in SEX_OPTIONS" :key="option" class="radio-wrapper">
              <input
                type="radio"
                name="sex"
                v-bind:value="option"
                v-on:change="changeSex(option)"
                v-bind:checked="option === form.sex"
              />
              {{ option }}
            </p>
          </div>
          <div class="form-group">
            <label for="name">名前</label>
            <input id="name" class="form-control" v-model="form.name" />
            <p>{{ form.name }}</p>
            <p>{{ formattedName }}</p>
          </div>
          <div class="form-group">
            <input type="submit" class="btn btn-primary" value="作成" />
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import _ from 'lodash';

type sexString = '女' | '男';
interface MemberInterface {
  name: string;
  sex: sexString;
  ordering: number;
}

@Component
export default class App extends Vue {
  public form: MemberInterface = {
    name: '',
    sex: '女',
    ordering: 0,
  };
  public members: MemberInterface[] = [];
  public ICONS = {
    男: 'fa-mars',
    女: 'fa-venus',
  };
  public SEX_OPTIONS: sexString[] = ['女', '男'];

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
  get formattedName(): string {
    if (!this.form.name) {
      return '';
    }
    return this.form.name + (this.form.sex === '女' ? 'ちゃん' : 'くん');
  }

  public changeSex(option: sexString): void {
    this.form.sex = option;
  }
  public save(): void {
    // orderingで席順を管理。新メンバーは最後尾
    const newMember = Object.assign({}, this.form, {
      ordering: this.members.length,
    });
    this.members.push(newMember);
    this.form.name = '';
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
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
