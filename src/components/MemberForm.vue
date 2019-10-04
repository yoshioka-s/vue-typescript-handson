<template>
  <form class="member-form" v-on:submit.prevent="save">
    <div class="form-group">
      <label for="sex">性別</label>
      <br />
      <p v-for="option in SEX_OPTIONS" :key="option" class="radio-wrapper">
        <input
          type="radio"
          name="sex"
          :id="option"
          v-bind:value="option"
          v-on:change="changeSex(option)"
          v-bind:checked="option === form.sex"
        />
        <label :for="option">{{ option }}</label>
      </p>
    </div>
    <div class="form-group">
      <label for="name">名前</label>
      <input id="name" class="form-control" v-model="form.name" />
    </div>
    <div class="form-group">
      <input type="submit" class="btn btn-primary" value="作成" />
    </div>
  </form>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { sexString, MemberInterface } from '@/interfaces/Member';

export default class MemberForm extends Vue {
  public form: MemberInterface = {
    name: '',
    sex: '女',
    ordering: 0,
  };
  public SEX_OPTIONS: sexString[] = ['女', '男'];
  public changeSex(option: sexString): void {
    this.form.sex = option;
  }
  public save(): void {
    this.$emit('addMember', this.form);
    this.form.name = '';
  }
}
</script>

<style lang="stylus" scoped>
.member-form {
  background-color: #e0e0e0;
  padding: 15px;
  border-radius: 15px;

  .radio-wrapper {
    display: inline-block;
    margin-right: 1em;

    input {
      margin-right: 0.5em;
    }

    &:last-of-type {
      margin-right: 0;
    }
  }
}
</style>