<template>
  <div id="app">
    <nav class="navbar navbar-light bg-light navbar-expand-lg">
      <a href="/" class="navbar-brand">
        <h1>席替え番長</h1>
      </a>
    </nav>
    <div class="row page">
      <div class="col-sm-8">
        <h2>メンバー {{ members.length }}人</h2>
        <ul>
          <li v-for="(member, i) in sortedMembers" v-bind:key="i">
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
    // フォームをリセット
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
  margin: 50px;
  color: #2c3e50;
}

.page {
  min-height: 300px;
}

.radio-wrapper {
  display: inline-block;
  margin-right: 1em;
}

.radio-wrapper:last-of-type {
  margin-right: 0;
}

.radio-wrapper input {
  margin-right: 0.5em;
}

.fa-mars {
  color: #03328b;
}

.fa-venus {
  color: #eb90c8;
}

.member-form {
  background-color: #e0e0e0;
  padding: 15px;
  border-radius: 15px;
}
</style>
