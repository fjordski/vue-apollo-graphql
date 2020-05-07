<template>
<div>
  <ul v-for='(todo, index) in todos' :key='index'>
    <li>{{ todo.text }}</li>
  </ul>
  <input type='text' v-model='newTodo'>
  <button @click='addTodo'>press me</button>
</div>
</template>
<script lang="ts">
import gql from 'graphql-tag';
import { Component, Prop, Vue } from 'vue-property-decorator';
// import ApolloExample from './ApolloExample.vue';

@Component({
  name: 'TestApollo',
  apollo: {
    todos: gql`query getTodos { todos { text }}`
  }
})

export default class HelloWorld extends Vue {
  @Prop() private msg!: string;

  newTodo = ''

  addTodo() {
    this.$apollo.mutate({
      // Query
      mutation: gql`mutation ($text: String!) {
        insert_todos_one(object: {text: $text}) {
          id
        }
      }`,
      // Parameters
      variables: {
        text: this.newTodo,
      },
    }).then(data => console.log(data))
  }
}
</script>
